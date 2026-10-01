CPU virtualization
- Process State
	- Running
		- 지금 수행 중인 상태
	- Ready
		- 준비는 되어있는데 다른 process가 processor를 쓰고있어서 실행은 안되고 있는 상태
	- Blocked
		- 수행될 준비가 안되어 있는 상태
		- e.g. disk로 data를 요청했는데 아직 못받았을 때 (I/O request)
- Data Structures
	- Kernel Process Structure
		- xv6 기준으로 ![[Pasted image 20260921005852.png]]
	- Process Control Block(PCB) 잠시 멈춰있던 Process를 다시 불러오기 위해서 필요한 것들
		- xv6 기준으로 ![[Pasted image 20260921005900.png]]
- How to efficiently virtualize CPU
	- Control
		- Process에게 CPU의 권리를 준 후에 어떻게 돌려받을 것인가?
		- Process가 Return을 주지 않으면? CPU로 이상한 짓을 하려고 하면?
	- Performance
		- Overhead를 어떻게 줄일 것이냐?

Limited Direct Execution
- 여러 문제에 의해서 우리는 Process가 모든 access를 얻는 것은 위험하다!
	- 이에 대한 해결책으로 user mode 와 kernel mode를 분리해야 한다. 
	- User mode
		- Applications do not have full access to hardware resources
	- Kernel mode
		- The OS has access to the full resource of the machine
- Syscall (System call)
	- 우리가 user mode에서 할 수 없는 일을 하기 위해서는 OS에 요청을 해야 하고, 이를 System call 이라고 한다. 
	- Trap instruction
		- Syscall이 발생해서 user mode에서 kernel모드로 들어가는 것
	- Return-from-trap instruction
		- 불렸던 user program으로 return하는 것
	- e.g.
		- open file example![[Pasted image 20260921155536.png]]
	- Trap Table
		- Syscall을 할 때 %eax에 syscall number를 저장하고, %ebx, %ecx, %edx, %esi, %edi에 aurguement를 담은 후 trap명령어의 syscall code를 포함하여 실행시킨다.
		- 이 중 syscall number에 대응하여 어떤 handler를 부를지를 결정하는 table을 Trap Table이라고 한다. 
- Kernel stack
	-  Kernel space로 진입할 때, user state(PC(%eip), stack pointer(%esp) 등)와 return addr, Local vars 등을 저장하기 위해서 User stack의 아래쪽에 Kernel stack을 둔다
	1. user stack 사용 중
	2. trap 발생
	3. kernel stack으로 이동
	4. syscall return
	5. user stack으로 돌아감
	- 먄약 user stack이 자라서 kernel stack에 닿으면 OS꺼짐 주의

Context Switch
- OS는 어떤 process를 정지시키고, 또 어떤걸 시작시킬지 정해야함
	- 만약 processs가 system call도 안하고 return도 안하면 어떻게 권한을 뺏어야 하나
- A cooperative Approach
	- process에서 알아서 syscall을 부르고 불렀을 때 return-from-trap을 하기 전에 switch할지 말지 정하자
- A Non-cooperative Approach
	- Hardware에 있는 timer를 이용해서 interrupt(IO가 발생시키는 trap)을 발생시켜서 특정 시간마다 강제로 syscall(interrupt handler)를 발생시킨다. 이때 switching을 결정한다. 
- 현재의 OS는 위 두 방법을 모두 사용한다. 
1. Scheduler에서 현재 process를 이어갈지 아니면 다른 것으로 바꿀지 정한다. 만약 바꾸기로 정했다면 OS가 context switch를 실행한다. 
2. Context switch는 low-level의 assembly code의 piece로 기존에 실행중이던 kernel register들(process structure)를 저장하고 실행 시킬 것을 불러온다. 
