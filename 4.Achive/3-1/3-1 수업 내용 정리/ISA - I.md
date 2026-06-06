
- How to specify what a computer does
	- Architecture (Instruction Set Architecture level)
		- Car: driving manual & operation manual
		- Computer: Instruction Set Manual 
	- Microarchitecture (implementation level)
		- A particular car design has a certain configuration of electrical and mechanical components
		- A particular computer design has certain configuration of datapath and control logic units

- Stored Program Architecture (von Newmann)
	- Stored-program architecture 
		- Instructions in a linear memory array
			- Instructions are in memory
		- Instruction can be modified just like data 
			- Both forms consist of 0s and 1s
	- Sequential instruction processing 
		1. Program Counter identifies the current instruction
		2. Instruction is fetched from memory and executed
		3. Program counter is advanced (according to instruction)
		4. Repeat

- von Neumann v.s. Dataflow
	- von Neumann: Consider a von Neumann program
		- von Neumann program은 실행 순서를 지키면서 실행한다. 
		- von Neumann bottleneck
			- 메모리의 읽고 쓰기가 많아서 작업의 한계가 존재한다. 이때 메모리의 병목 현상이 발생할 수 있다. 
	- Dataflow: Instruction ordering specified by dataflow dependence (no PC)
		- Each instruction specifies who should receive results
		- An instruction can execute "whenever" all operands are received

- What are specified/decided in an ISA
	- Any information that should be given to a (machine-level or assembly) programmer
	- Data flow, Memory Model, Program Visible State, Instruction Set, System Model, External Interface
- Program Visible State
	- Memory: Array of storage locations indexed by an address
	- Register: Given special names in the ISA
	- Program counter: Memory address of the current instruction

- General Instruction Classes
	- Arithmetic and logical operations
		- In general, can be categorized into integer and floating-point instructions
		1. Fetch operands from specified locations
		2. Compute a result as a function of the operands
		3. Store result to a specified location
		4. Update PC to the next sequential instruction
	- Data movement operations
		1. Fetch operands from specified locations
		2. Store operand values to specified locations
		3. Update PC to the next sequential instruction
	- Control flow operations
		1. Fetch operands from specified locations
		2. Compute a branch condition and a target address
		3. If "branch condition is true"
			1. then PC = target address
			2. else PC = next sequential instruction

- Atomicity of an Instruction
	- Every instruction is either completely executed or not executed at all at any moment, from the SW perspective
	- In other words, partial updates to the programmer visible state by an instruction should NEVER be observed
	- Later, we will also cover 'atomic RMW(read-modify-write) instructions'

 - Evolution of Register Architecture
	 - Accumulator $\rightarrow$ Accumulator + address registers $\rightarrow$ General purpose register
	 - Accumulator + address registers
		- Need register indirection
		- Initially address registers were special-purpose (i.e. only used to hold address for indirection)
		- Eventually arithmetic on address registers supported
	- General purpose registers
		- All registers good for all purposes
		- Ranges from a few registers to 32(common for RISC) to 128

- Operand Source
	- Number of operands: Monadic, Dyadic, Triadic
- Memory Addressing Modes
	- Absolute (LD rt, 10000): use immediate value as address
	- Register Indirect (LD rt, ($r_{base}$)): use GPR\[$r_{base}$] as address
	- Displaced or based (LD rt, offset($r_{base}$}): use offset + GPR\[$r_{base}$] as address
	- Indexed (LD rt, ($r_{base}, r_{index}$)): use GPR\[$r_{base}$] + GPR\[$r_{index}$] as address
	- Memory Indirect (LD rt (($r_{base}$))): use value at M\[GPR\[$r_{base}$]] as address
	- Auto inc/decrement (LD rt, ($r_{base}$)): use GPR\[$r_{base}$] as address, but increment or decrement GPR\[$r_{base}$] each time
	- Anything else you can think of ...

- RISC (Reduced Instruction Set Computer)
	- Simple operations
		- 2-input, 1-output arithmetic and logical operations
		- Few alternatives exist to do the same thing
	- Simple data movements
		- ALU ops are register-to-register
		- Memory can be accessed by only load adn store instructions $\rightarrow$ "Load-store architecture"
	- Simple branches
		- Limited varieties of branch conditions and targets
	- Simple instruction encoding
		- All instructions encoded in the same number of bits
		- Only few formats
		- instruction이 뭐고, input/output이 뭔지 판단하는데 필요한 시간을 아낌

- Evolution of ISA
	- Why were the earlier ISAs so Simple?
		- Technology limitation
		- Inexperience, lack of precedence
	- Why did it get so complicated later (CISC - Complex Instruction Set Architecture)
		- Assembly programming
		- Lack of memory size and performance
		- Microprogrammed implementation
	- Why did it becomes simple again (RISC)
		- Memory size and speed(cache)
		- Compliers
	- Why x86 is still "king of the hill"
		- Technology vs. economics
		- Technology vs. psychology
		- Technology vs. deep pocket