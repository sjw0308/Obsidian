
Multiprocessor 에서 programmer가 고려할 점
- 어떻게 작업을  나눌지
- Data splitting
- A의 연산 결과가 B로 들어감 = A 실행 후 B 실행해야 됨: Data dependency
- Testing and debugging 등등

Concurrency: 우리가 보기에 동시에 실행 (single processor도 가능)
Parallelism: 실제로 동시에 실행(multi processor에서만)
Type of Parallelism
- Data parallelism
	- Data를 쪼개고 각 sub-data에 cpu를 배정해서 같은 작업을 실행
- Task parallelism
	- Data는 하나 + 전체 데이터에 대해 A작업과 B작업을 나눔
- Hybrid parallelism
Amdahl's Law
- S를 serial portion (lock 등으로 인해서 병렬적으로 실행이 불가능한 부분의 비율)이라고 하고 N을 # of processing core라고 하면
- $\text{speed up} \leq \frac{1}{S+\frac{1-S}{N}}$
- 어쩌면 process를 늘리는 것보다도 serial portion을 줄이는게 중요하다

Multiprocessor Scheduling Background
- cpu에는 cache와 memory가 연결되어 있다.
- multicore는 memory는 share하지만 cache는 각자 가지고 있다고 가정한다
	- 그래서 Cache coherence 문제가 발생한다 
	- 이는 보통 hardware적으로 해결한다
- multicore 때문에 발생할 수 있는 synchronization 문제가 있음으로 lock을 더 신경써야 한다. (free 등등을 확인)
- Cache Affinity
	- 프로세스 A를 CPU0에서 실행했으면 A는 계속 CPU0에서 실행하는게 좋다. 

Multiprocessor Scheduling Algorithms
- Single Queue Multiprocessor Scheduling (SQMS)
	- 모든 job을 하나의 single queue로 관리 (각 CPU가 다음 작업을 찾을 때 해당 Queue에서 기존 배웠던 scheduling policy에 맞게 다음 task를 뱆정)
	- 장점: 구현 쉬움
	- 단점: lock을 많이 걸어야 해서 scalable하지 않고 cache affinity를 높이기 힘듦
- Multi-Queue Scheduling (MQMS)
	- CPU별로 하나씩 Queue를 둠 (다른 CPU에서 해당 queue에 접근 불가 -> lock free = scalable) -> 이제 각 작업을 어디에 배정할지만 결정하면 됨
	- 작업이 적은 CPU에 배정해도 먼저 끝나는 CPU발생 가능
		- 원칙과는 위배되지만 예외적으로 작업을 옮기는 Migration으로 해결
		- Work stealing: CPU가 옆 놈의 queue를 보고 일을 가져오게 하기 (scability때문에 계속 볼 순 없음)
