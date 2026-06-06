- RISC-V Programmer Visible State
	- Also referred to as "Architectural state"
	- PC, General-Purpose Register File, Memory
	- 이때 General-Purpose Register 중 x0는 항상 0의 값을 갖는다. 

- Data Format
	- word(32-bit), byte(8-bit), ...
	- Floating-point numbers - Single(32-bit) or Double(64-bit) precision

- RISC-V Instruction Format
- ![[Pasted image 20240324160308.png]]
	- Why a separate format for Store and Load?
		- 다른 instruction 들과 format을 맞추는 것이 효율이 좋음

- Endian
	- Big Endian
		- MSB가 앞(memory address상)에 나오도록 하는 바이트 순서
	- Little Endian
		- MSB가 뒤(memory address상)에 나오도록 하는 바이트 순서
	- RISC-V는 Little endian을 사용한다. 
- Data alignment in memory
	- word boundary에 따라 데이터를 정렬하는 것
	- RISC-V에서는 unaligned data access 또한 허용하지만 느림
	- RICS-V에서는 unaligned instruction access는 허용되지 않음
	- 보통은 aligned access가 기본적이다. 

- Register - Register ALU instruction (R-type)
	- Encoding
		- funct7(7 bits), rs2(5 bits), rs1(5 bits), funct3(3 bits), rd(5 bits), opcode(7 bits, 0110011)
	- Semantics
		- GPR\[rd] $\leftarrow$ GPR\[rs1] (OP) GPR\[rs2]
		- PC $\leftarrow$ PC + 4 (word 크기)
	- ADD, SUB와 같은 arithmetic, SLT 같은 compare, AND 같은 logical, SLL 같은 shift가 여기 포함된다. 
	- Exception: none

- Register - Immediate ALU Instruction (I-type)
	- Encoding
		- imm\[11:0] (12 bits), rs1(5 bits), funct3(3 bits), rd(5 bits), opcode(7 bits, 0010011)
	- Semantics
		- GPR\[rd] $\leftarrow$ GPR\[rs1] (OP) sign-extend(immediate)
		- PC $\leftarrow$ PC + 4
	- R-type에서 imm이 추가된 형태이지만 SUBI는 없다. 
	- Shift instruction을 할 때는 imm의 뒤 5 bits를 크기로, 그 앞의 7 bits 로는 logical shift인지 arithmetic shift인지 구분한다. 그 이유는 32 bits 이상 shift를 할 필요가 없기 때문이다. 
	- Exception: none

- Upper immediate format (U-type) (lui)
	- Encoding
		- imm\[31:12] (20 bits), rd(5 bits), opcode(7 bits, 0110111)
	- Semantics
		- GPR\[rd] $\leftarrow$ imm + 0's
		- PC $\leftarrow$ PC + 4
	- 다른 type에서 imm이 32 bits가 아니기 때문에 imm의 크기가 부족할 경우 사용한다. 20 bits의 imm을 constant 값의 \[31:12]에 저장하고 뒤의 \[11:0]을 0으로 만든 constant 값을 rd에 저장한다. 

- Load Instructions (I-type)
	- Encoding
		- imm\[11:0] (12 bits), rs1(5 bits), funct3(3 bits), rd(5 bits), opcode(7 bits, 0000011)
	- Semantics
		- byte_address = sign-extend(offset) + GPR\[base(=rs1)]
		- GPR\[rd] $\leftarrow$ MEM\[byte_address]
		- PC $\leftarrow$ PC + 4
	- Exceptions will be discussed later
	- LW (word), LH (halfword), LB (byte) 와 같이 sign-extend를 하는 load들 그리고 LHU, LBU와 같이 zero-extension을 하는 load가 있다. 

- Store Instructions (S-type)
	- Encoding
		- imm\[11:5] (7 bits), rs2(5 bits), rs1(5 bits), funct3(3 bits), imm\[4:0] (5 bits), opcode(7 bits, 0100011)
	- Semantics
		- byte_address = sign-extend(offset) + GPR(base(=rs1))
		- MEM\[byte_address] $\leftarrow$ GPR\[rs2]
		- PC $\leftarrow$ PC + 4
	- Exceptions will be discussed later
	- SW (word), SH (halfword), SB (byte)

- Conditional Branch Instructions (SB-type / B-type)
	- Encoding
		- imm\[12, 10:5] (7 bits), rs2(5 bits), rs1(5 bits), funct3(3 bits), imm\[4:1, 11] (5 bits), opcode(7 bits, 1100111)
	- Semantics
		- target = PC + sign-extend(imm)
		- If GPR \[rs1] (compare) GPR\[rs2]
			- then PC $\leftarrow$ target
			- else PC $\leftarrow$ PC + 4
	- BEQ, BNE, BLT, BGE: signed variations / BLTU, BGEU: unsigned variations

- Jump Instructions (UJ-type / J-type) (JAL)
	- Encoding
		- imm\[20, 10:1, 11, 19:12] (20 bits), rd(5 bits), opcode(7 bits)
	- Semantics
		- target = PC + sign-extend(imm)
		- GPR\[rd] $\leftarrow$ PC + 4
		- PC $\leftarrow$ target
	- Exception: misaligned target

- Jump Indirect Instruction (I-type) (JALR)
	- Encoding
		- imm\[11:0] (12 bits), rs1(5 bits), funct3(3 bits), rd(5 bits), opcode(7 bits)
	- Semantics
		- target = GPR\[rs1] + sign-extend(imm)
		- target &=0xFFFFFFFE (for aligning)
		- GPR\[rd] $\leftarrow$ PC + 4
		- PC $\leftarrow$ target
	- Exception: misaligned target (4-byte)

- Caller and Callee saved registers
	- Callee saved register
		- Callee가 기존 값을 변경하지 않은 상태로 return해야만 함
	- Caller saved register
		- Callee가 값을 변경할 수도 있기 때문에 Caller가 해당 값을 미리 저장한 후 Callee를 불러야만 함

- Single program memory usage convention
	- Stack space 
		- Automatic storage
	- Free space
		- stack grows down and dynamic data grows up
	- Dynamic data
		- Heap
	- Static data
		- Global variables
	- Text
		- Program code
	- reserved