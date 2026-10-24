
What is Deadlock
- 두개 이상의 thread나 process가 서로가 끝나기를 기다리는 상태가 끝나지 않는 것
Deadlock Characterization
- 아래 4개를 모두 만족시켜야 deadlock이 발생
1. Mutual exclusion
	- thread가 lock을 잡는 등의 mutual exclusion상황이 있어야함
2. Hold and wait
	- thread가 resource를 잡고 additional resource를 기다려야함
3. No preemption
	- 이미 잡고 있는 resource를 놓지 못해야함
4. Circular wait
	- thread사이에 서로 가진 resource를 기다리는 circular chain이 있어야 함
Method for handling Deadlock
- 프로그램에서 Deadlock이 발생하지 않게 막기
	- Deadlock prevention (static): 프로그램을 짜는 단계에서 deadlock이 생기지 않게 하기
	- Deadlock avoidance (dynamic): 프로그램에서 deadlock이 발생할 가능성이 있으면 OS가 피해감
- Deadlock에 들어가는 건 허용하지만 이를 발견하고 해소하기
	- Deadlock detection & recovery
- Deadlock 무시
	- 현재 대부분의 OS가 택하는 방법
Deadlock Prevention
- deadlock 발생 조건 1~4 중 하나만 막으면 됨
- Circular wait
	- 모든 lock에 순서를 주고 lock을 잡는 순서를 맞추기
- Hold and Wait
	- 모든 lock을 atomically잡음(잡는 lock들을 전부 포함하는 lock을 잡기)
	- concurrency에 큰 issue.. 그러나 디버깅할때는 유용하다
- No preemption
	- trylock함수를 만들어서 내가 실패하면 이전에 잡고 있던 lock을 풀고 이전 lock을 잡는 곳으로 돌아가도록 만듦
	- 문제점
		- live lock: 서로 lock하나를 잡았다가 풀었다가를 반복하고 나아가지 못하는 상황이 가능. 이는 이전에 잡고 있던 lock을 풀 때 random delay를 주어서 해결가능
		- 첫번째 lock과 두번째 lock사이에 코드가 너무 많으면? malloc잡은 것도 free해야되고 해당 부분을 또 반복해야되고, 손해가 너무 많음
- Mutual Exclusion
	- 아래 CompareAndSwap함수 등을 사용해서 race condition없이(lock 없이) 프로그램을 짜자
	- ```C
	int CompareAndSwap(int *address, int expected, int new){
	    if (*address == expected) {
	        *address = new;
	        return 1; // Success
	    }
	    return 0; // Failure
	}
	```
	- 아직은 모든 작업에 이게 적용되지는 않음
Deadlock Avoidance
- Deadlock Prevention은 절대 deadlock이 발생하지 않는다는 strong guarantee를 주지만 Deadlock Avoidance는 mild guarantee를 제공한다. 
- 몇몇 lock에 대한 global knowledge를 필요로 한다. 
- deadlock이 발생할 가능성이 있는 프로그램 자체를 동시에 실행하지 않음
- 문제점
	- concurrency이슈 + 이미 프로그램을 잘짜서 deadlock이 없어도 이를 알 수가 없어서 성능저하기 일어남

Deadlock Detection & Recovery
- 걍 가끔 Deadlock이 일어나면 일어나라고 하고 더 가끔 rebooting함