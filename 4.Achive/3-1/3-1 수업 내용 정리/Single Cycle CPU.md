- CPU를 하나의 FSM으로 볼 수 있다. 
	- State == Program visible state
	- Next-state logic == instruction execution
	- One state transition per instruction in abstract FSM -> ISAs atomic instruction semantics
- Programmer-Visible state
	- PC
		- input: next PC value
		- output: PC value
	- Registers
		- input: Register numbers (read register 1, read register 2, write register), Data (write data)
		- output: Data (read reg data 1, read reg data 2)
		- signal: RegWrite
	- Instruction memory
		- input: Instruction address (PC value)
		- output: Instruction
	- Data memory
		- input: Address, Write data
		- output: Read data
		- signal: MemWrite, MemRead

- Magic Register file and Memory
	- Combinational read
		- The output of the read data port is a combinational function of the register file contents and the corresponding read select port
		- Similarly, instruction/data memory work like combinational logic
	- Synchronous write
		- The selected register (or memory location) is updated only on the positive-edge clock transition when write enable is asserted
		- Cannot affect read output in between clock edges
	- Now, consider the instruction memory as read-only memory

- Instruction processing
	- IF: Instruction fetch
		- Instruction memory로 부터 instruction을 읽어옴
	- ID: Instruction decode and operand fetch
		- instruction을 분석(decode)해서 어떤 작업을 해야 하는지 알아냄
		- 필요한 operand register들을 읽어옴
	- EX: ALU/execute
		- ALU 등에서 계산을 진행
	- MEM: (Data) Memory access (only for load & store instructions)
		- instruction에서 접근하려고 하는 메모리에 접근하여 작업 (read or write)를 진행
	- WB: Write-back
		- update register value
		- update destination value

- R-type Instruction Datapath
	- PC $\rightarrow$ Instruction memory, ALU(for next PC = PC + 4)
	- Instruction mem에서 instruction을 읽음
	- instruction $\rightarrow$ parsing(by decoder)
	- parsed instruction $\rightarrow$ Register file $\rightarrow$ read reg data 1, 2
	- reg data 1, 2 $\rightarrow$ ALU $\rightarrow$ ALU result
	- ALU result $\rightarrow$ register file Write data
	- ![[SmartSelect_20240329_173117_Flexcil.jpg]]

- I-type ALU Instruction Datapath
	- Immediate generator before ALU and select input between imm and Read data 2
	- ![[SmartSelect_20240329_173656_Flexcil.jpg]]

- I-type Load Instruction Datapath
	- ALU result $\rightarrow$ data memory address $\rightarrow$ Read data
	- Read data $\rightarrow$ extension $\rightarrow$ select Write data of register file
	- ![[SmartSelect_20240329_174104_Flexcil.jpg]]

- S-type Instruction Datapath
	- Connect Read register data 2 to Write data of Data memory and some control signals
	- Do not Write back a data to register if Store instructions
	- ![[SmartSelect_20240329_174615_Flexcil.jpg]]

- J-type Instruction Datapath
	- Additional adder for add PC and Imm
		- Before add Imm to PC, shifting left to Imm by one bit 
	- Choose PC + 4 or PC + Imm for update PC
	- Connect PC + 4  to Write data by Mux
	- ![[SmartSelect_20240329_174943_Flexcil.jpg]]

- I-type JALR Instruction
	- Connect ALU result to update PC by Mux
	- ![[SmartSelect_20240329_175437_Flexcil.jpg]]

- B-type Instruction
	- Make ALU gives another output which is about branch condition named bcond
	- If bcond is true then PC Src1 Mux's 2nd input be the next PC
	- else PC + 4 be the next PC
	- Register Write-back must be disabled
	- ![[SmartSelect_20240329_192521_Flexcil.jpg]]

- Datapath with Control
	- 위 final datapath 에서 여러 signal들을 하나의 control에서 관리한다. 이때 각 signal은 ALUOp를 제외하고는 instruction의 operation에 따라 달라진다. 
	- 따라서 ALUOp를 제외한 나머지 signal은 opcode를 입력으로 받는 Control로 묶어서 조종한다. 
	- ALUOp는 signbit, funct3, funct7을 입력으로 하는 ALU control로 조종한다. 
	- ![[SmartSelect_20240329_193015_Flexcil.jpg]]
	- RegWrite - Store, Branch가 아닐 경우 1
	- ALUSrc - R-type, B-type이 아닐경우 1
	- MemRead - Load일 경우 1
	- MemWrite - Store일 경우 1
	- MemtoReg - Load일 경우 1
	- PCtoReg - JAL 또는 JALR일 경우 1
	- PCSrc1 - 위의 Gate 참고
	- PCSrc2 - JALR일 경우 1
