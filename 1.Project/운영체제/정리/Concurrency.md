
Thread
- Process내에서 여러 execution을 동시에 실행하게 만들어주는 개념
- Process의 process라고 생각하면 된다
- PC가 여러개이다
- Context Switch between Threads
	- 각각의 Thread는 각자의 PC와 Register set을 가진다
	- 그렇지만 같은 virtual memory와 file system을 갖는다
	- 그래서 Process control blocks와 같이 현재 상태를 저장하는 Thread control block(TCB)가 존재한다.
	- 근데 thread끼리 바뀔 때는 address space는 남는다는 점이 다르다

Concurrency issue
- Thread끼리는 같은 address space에 접근하기 때문에 여러 문제가 발생한다
- 그래서 발생하는 상황을 Race Condition이라고 하고, 그런 Race condition이 발생할 수 있는 코드 내의 부분을 Critical section이라고 한다
- Enforcing Mutual Exclusion
	- 특정 코드 구역에 하나의 thread가 실행 중이면 다른 thread는 실행하지 못하게 하는 방법
	- 그래서 우리는 thread의 execution을 synchronize 해야하고 특정 critical section을 한번에 실행(all or not / atomically)하게 만들어야 한다. 
	- lock과 semaphore를 사용한다. 
