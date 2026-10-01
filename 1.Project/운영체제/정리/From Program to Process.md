
CISC vs. RISC
- CISC(Complex instruction set computer): Hardware에 복잡한 연산도 넣자
- RISC(Reduced instruction Set Computer): Hardware에는 단순한 연산만 넣고 컴파일러(시스템)을 잘 만들자. 최근 휴대용 device는 베터리의 한계가 있기 떄문에 더 주목받고 있음

Architecture
- The parts of a processor design that one needs to understand to write assembly code
- assembly가 보는 CPU
- Instruction Set Architecture(ISA)와 같은말
Microarchitecture
- Implementation of the architecture

Programmer-Visible state
- PC(Program Counter)
	- Address of next instruction
	- Called "EIP" or "RIP"
- Register file
	- Heavily used program data
- Condition codes
	- Store status information about most recent arithmetic operation
	- Used for condition branches
- 폰노이만 아키텍처
	- CPU $\leftrightarrow$ Memory

Turning C to executable
1. C program (main.c) (text) $\xrightarrow{compiler}$ (이때 gcc -s 사용)
2. Assembly program (main.s) (text) $\xrightarrow{Assembler}$ (이때 gcc -c 또는 as 사용)
3. Object program (main.o) (binary) $\xrightarrow{Linker}$ (이때 gcc 또는 ld 사용, static libraries를 불러옴)
4. executable program (p)

Executable Object File Format (elf format)
1. ELF header
	- Byte ordering이나 file type, machine type 등등
2. .text section
	- Code
3. .rodata section
	- Read Only Data: jump table for switch 등등
4. .data section
	- Initialized global variables
5. .bss section
	- Uninitialized global variables
6. 등등

Loading executable
- executable을 main memory에 올릴 때 ...
- ![[Pasted image 20260921002538.png]]

Process
- an instance of a running program
- Process에는 두 가지의 중요한 abstraction을 제공해야 한다. 
	- Logical control flow
		- CPU virtualization concept
	- Private virtual address space
		- Memory virtualization concept
- APIs
	- Create: Create a new process to run a program
		- fork(): 새로운 프로세스 만들기
		- execv(): 새로운 프로그램으로 프로세스 덮어쓰기
	- Destroy: Halt a runaway process
		- exit(), kill(SIGKILL)
	- Wait: Wait for a process to stop running
		- wait(), waitpid()
	- Miscellaneous Control: Some kind of method to suspend a process and then resume it
		- kill(SIGSTOP), signal()
	- Status: Get some status info about a process
		- getpid(), getpriority(), ...
- Creation
	1. Load a program into memory, creating the address space of the process (Dynamic loading: 모든 프로그램을 한 번에 load하지 않고 프로그램을 실행)
	2. The program's run-time stack is allocated
	3. The program's heap is created
	4. The OS does some other initialization tasks
	5. Start the program running at the entry point, main()
		- 여기까지 CPU의 권한을 잡은 것은 OS이고, main실행 이후부터는 CPU의 권한을 잡는 것은 프로그램이다.
	- 이후 Process 실행

- CPU Control flow
	- 실제로 CPU는 한번에 하나의 instruction밖에 실행을 못하지만 여러개의 task의 logical flow가 동시에 실행되는것 같이 보이게 함
	- CPU virtualization
		- Mechanism: Context Switch
		- Policy: Scheduling Policy
	- 이때 task가 아닌 것들에 시간이 쓰이는데 이걸 Overhead라고 한다. 
	- ![[Pasted image 20260921004715.png]]
	- 위 사진에서 
		- Concurrent: A&B, A&C. A->B->A 와 C->A->C로 실행됬기 때무에 동시에 실행하는 것 처럼 보인다. 
		- Sequential: B&C. 위처럼 겹치지 않아서 B가 실행된 후 C가 실행되는 것처럼 보인다. 
