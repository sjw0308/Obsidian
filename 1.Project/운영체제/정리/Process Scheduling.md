
Basics of Scheduling Policies
- 우리가 최소화 해야하는 것은 작업부화(workload)이다.
- 초기 작업부화 가정
	1. 모든 작업은 같은 시간이 걸린다. 
	2. 모든 작업은 같은 시간에 시작된다.
	3. 모든 작업은 CPU만 사용한다 (IO없음)
	4. 모든 작업의 필요시간을 알고 있다. 
- Performance metric
	- Turnaround Time = T(완료 시각) - T(작업이 온 시각)
	- Fairness
- First in, First out
	- 먼저 온 작업을 먼저 실행
	- A,B,C가 와서 각각 10초씩 걸려서 완료된다면
	- Average turnaround time = (10+20+30)/3 = 20
	- 1번 가정을 없애면 긴 시간 걸리는게 먼저 왓을 때 평균이 엄청 길어지는 문제가 생김 = convoy effect
- Shortest Job First
	- job을 입력받고 잠깐 기다린 후 job중 가장 짧은 걸 먼저 실행
	- 2번 가정을 없애면 짧은 작업이 기다림이 짧은 시간 이후에 들어왔을 떄 위의 방법과 같은 문제가 발생함
- Shortest Time-to-Completion First
	- 새로운 job이 들어올 때 마다 가장 빨리 끝나는 job이 CPU를 선점
	- 그러나 해당 모델은 time-shared machines에는 맞지 않음(ex. notepad같은 interactive한 job은 전체가 끝나는 시간보다 조금이라도 response를 빨리 받는게 중요함)
- New metric
	- Response Time = T(결과가 처음 나오는 시간) - T(작업이 온 시간)
- Round Robin Scheduling
	- 우리가 임의로 정한 Time slice에 따라 다음 queue에 있는 job으로 Switch
	- RR(Round Robin)은 Fair하지만(Response time은 짧지만) Turnaround time을 기준으로는 성능이 나쁨
	- A,B,C가 와서 각각 10초씩 걸려서 완료된다면 -> Average turnaround time $\sim$ 30
	- Time slice를 잘 잡는 것이 성능에 critical함. 짧으면 response time은 짧아지지만 overhead가 증가, 길어지면 반대
- IO발생
	- IO가 완료될 때까지 해당 job을 block하고 다른 job을 schedule해야됨.
	- 이후 IO가 완료되면 interrupt를 발생시켜서 다시 해당 job을 ready state로 바꿔야함

Practical Scheduling Policies
- Multi-Level Feedback Queue
	- 현실은 batch(사람과 interact가 별로 없는 작업)과 interactive job의 혼합이고 이를 우리가 선제적으로 알 수 없다. 
	- MLFQ는 STCF와 RR을 잘 섞은 방법과 비슷하다. 
	- Basic Rules
		1. higher queue에 있는 작업이 우선적으로 실행된다. 
		2. 같은 priority의 작업들은(같은 queue에 있는 작업들은) RR로 실행된다. 
		3. 처음 들어왔을 땐 가장 높은 priority의 queue로 배정한다. 
		4. 한 Time slice동안 해당 job이...
			1. IO없이 CPU만 사용시 interactive한 job이 아니라고 판단하고 우선도를 낮춘다
			2. IO가 발생하면 우선도를 유지한다. 
	- Problems
		- 우선순위가 높은 작업이 많으면 낮은 작업은 거의 실행되지 않음
		- 어떤 job은 시간에 따라 interactive job으로 바뀔 수도 있는데 priority를 높일 방법이 없음 (중간중간 모든 job의 priority를 가장 높은 queue로 바꾸는 priority를 사용하므로써 해결할 수 있다)
		- 임의로 정해야 하는 parameter가 너무 많다. (queue의 개수, time slice의 길이, priority boost의 주기인 S 등등)
	- Time slice
		- 높은 순위의 queue는 short time slice를 가져가고, 낮은 우선순위의 queue는 보통 batch작업이므로 더 긴 time slice를 가져간다. 
- Proportional share(or fair-share) scheduler
	- 각 작업이 일정 percentage의 CPU time을 가질 수 있도록 보장하는 방법들이다. 
	- Basic Concept: Ticket을 주고 각 ticket의 percentage를 system resource를 얼마나 가질 수 있는지에 대응시킴
- Lottery Scheduling
	- 각 Process들이 ticket을 나눠갖고, Scheduler가 uniformly random하게 winning ticket을 하나 뽑아서 그 숫자에 해당하는 작업을 수행한다. 
	- Ticket Currency
		- 각 작업은 자식 process를 생성할 때 자신만의 ticket을 배분하여 자신의 작업을 어떻게 나눌지 정할 수 있다. 
	- Ticket Transfer
		- Process가 임시적으로 자신의 ticket을 다른 process에 전달할 수 있다. 
	- Ticket Inflation
		- Process가 자신의 ticket개수를 늘리거나 줄일 수 있다. 
	- Implementation
		- random number generator: 하드웨어에서 제공됨
		- data structure to track the processes: linked list를 주로 사용
		- The total number of tickets: 변할 수 있지만 초기 값을 정해야 함
	- Fairness metric
		- fairness를 어떻게 측정할 것인가?
		- U = (job 1번의 총 수행시간) / (job 2번의 총 수행시간)
- Stride Scheduling
	- 결정적으로 fair-share scheduler임
	- 모든 process들이 stride와 pass-value를 가진다.
	- scheduler는 가장 낮은 pass-value의 process를 실행하고, 해당 process의 pass-value를 stride만큼 증가시킨다. (같은 pass-value끼리는 랜덤)
		- ticket이 많다 = stride가 작다
	- 초기 pass-value는 0으로 초기화
		- 여기서 문제가 발생한다. 다른 process들이 실행되던 중에 새로운 process가 들어오면 한동안 그 process만 수행될 것이다. 
		- 이를 해결하기 위해서 새로운 process가 들어오면 모든 process의 pass-value를 0으로 초기화 하거나, 초기 pass-value를 가장 낮은 pass-value와 같게 세팅하는 등의 작업이 필요하다.
	- vs lottery scheduling
		- lottery scheduling은 운에 의존해야하는 것에 반해서 우리가 정해서 fair함을 유지할 수 있다는 장점이 있지만 보통 lottery를 많이 사용함
		- calibrating global states is difficult <<= 이게 원인 중 하나
- Completely Fair Scheduler
	- 지금까지 scheduler의 overhead는 O(n)이었다. 그리고 이런 overhead를 줄이는게 key goal 중 하나이다. 여기서 다룰 Linux Completely Fair Scheduler(CFS)는 scalable한 것이 특징이다.
	- Vruntime: Process가 실행되면 일정비율로 늘어나는 virtual runtime. CFS는 Vruntime이 가장 작은 process를 실행한다.
	- sched_latency
		- 유동적인 time slice를 위한 재료
		- 모든 process는 sched_latency를 process의 개수 n으로 나눈 값을 time slice로 한다. 즉, 모든 process는 sched_latency동안 적어도 한 번은 수행된다. 
		- sced_latency는 우리가 정해야되는 parameter
		- 그러나 process가 너무 많아지면 overhead가 너무 늘어난다
			- 해결법으로는 min_granularity를 두면 된다. 
		- min_granularity: time slice의 minimum값, process가 너무 많아지면 sched_latency값이 늘어나는 효과가 된다.
	- nice_value
		- process의 priority를 나타내는 값으로 높은 priority(-20) ~ 낮은 priority(+19)사이의 값을 가질 수 있음
		- 각 nice_value에 대한 weight가 있고, 이는 vruntime을 update할 때 사용된다. 
		- 이 또한 parameter이고, nice value가 낮을 수록 큰 weight를 가진다. 
		- $\text{vruntime}_{i}=\text{vruntime}_{i}+\frac{\text{weight}_{0}}{\text{weight}_{i}}\cdot\text{runtime}_{i}$
			- 위 식에서 보면 높은 우선순위 -> 낮은 nice value -> 큰 weight (weight_0 대비) -> 실제 runtime보다 낮게 vruntime이 늘어남 -> 더 먼저(자주) 실행됨
	- CFS는 process들을 vruntime을 기준으로 red-black tree (balanced binary tree)로 관리되기 때문에 다음 process를 선택할 때 O(log n)으로 선택할 수 있다. 
		- [red-black tree](https://namu.wiki/w/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99%20%ED%8A%B8%EB%A6%AC)
	- IO가 많이 발생하는 process (interactive한 job) -> real-runtime이 작음 -> vruntime또한 작음 -> 더 자주 선택됨
	- Starvation
		- 새로 들어온 job의 vruntime이 0이라서 발생할 수 있음
		- 새로 job이 추가될 때 vruntime을 현재 process의 minimum value로 설정한다. 
