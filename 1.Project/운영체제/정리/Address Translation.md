
Design Goals
- Transparency: 돌아가는 프로그램이 가상 환경임을 알 수 없어야 한다
- Protection: 다른 프로세스가 접근하는 것을 막아야 한다
- Flexibility: 쉽게 만들 수 있어야 한다
- Efficiency
	- Time efficiency: 프로그램이 너무 느려지지 않아야 한다
	- Space efficiency: physical memory를 낭비하지 않아야 하고, 너무 많은 공간을 virtualization에 사용하지 않아야 한다 

Address Translation은 언제 해야될까?
- Compile time
	- Compile time에 address translation은 compiler가 physical address와 directly mapping하는 것이므로 virtual address=physical address인 것이다. 
	- 그렇다면 하나의 process에서 해당 프로그램을 실행중이라면 다른 process에서 해당 프로그램을 실행할 수 없다 (같은 주소 영역을 사용해야 하기 때문)
- Load Time
	- Load time에 translation을 한다면 메모리를 불필요하게 크게 잡거나 다른 process의 memory에 접근할 가능성이 존재한다
- Execution time
	- CPU에서 실행하면서 변환하는 방법으로 추가적인 연산이 Runtime(Execution time)에 필요하다는 단점이 있다. 
Address translation with HW
- CPU안에 있는 MMU(Memory Management Unit)을 통해서 virtual to physical address를 runtime에 mapping한다. 

Dynamic relocation
- Two HW registers
	- Base register: start address를 저장하는 register. program이 loading될 때 결정된다
	- Bound register: memory의 길이를 저장하는 register
- address translation 방법
	- 모든 프로그램은 compile에서는 address 0을 기준으로 만들어 진다. 
	- 프로그램이 시작될 때 OS가 base, bound register를 결정한다.
	- 실행 중 memory reference가 실행되면 MMU에서 physical memory로 변환하여 physical memory에 접근한다. 
- example
	- instruction에 의해서 memory 접근을 함
	- MMU에 해당 virtual memory 주소를 넘김
	- 해당 주소가 bound register보다 작은지 확인함
	- virtual memory에 base register를 더해서 physical memory를 얻음
	- ![[Pasted image 20261115202541.png|281]]
- 3가지의 중요 이슈
	- 프로세스가 시작할 때 가능한 base를 찾아야 한다. 
		- free list로 빈 physical memory의 시작 주소를 관리함. 프로세스가 생성되면 해당 리스트에서 하나를 꺼내서 쓴다.
	- 프로세스가 끝날 때 physical memory를 다시 쓸 수 있게 만들어야 한다. 
		- 프로세스가 끝나면 위의 리스트에 추가한다. 
	- context switch가 일어났을 때 base, bound register를 저장해두어야 한다. 
		- PCB 안에 보관한다
- 문제
	- virtual memory의 heap과 stack사이에는 큰 free chunk가 존재해서 전체 크기가 physical memory보다 더 크다. 
