
Replacement policy
- 어떤 page를 main memory에서 swap space로 evict할지를 결정하는 policy
- EAT를 줄이는 방법은 Page fault time을 줄이거나 p를 줄여야 하는데 전자는 HW의 변화가 필요하고 후자는 Replacement policy를 잘 설정하여 줄일 수 있다. 
- Optimal Replacement Policy
	- 앞으로 접근되는 모든 page를 알고 있는 이상적인 경우 가장 늦게 다시 사용되는 page를 replace하는 것이 optimal replacement policy이다. 
	- OPT를 우리의 policy를 평가하는 reference로 활용한다. 
- FIFO
	- 먼저 들어온 page를 먼저 evict
	- Belady's Anomaly: FIFO를 사용했을 때 Main Memory의 크기가 커졌는데 Page fault도 같이 늘어나는 것을 의미한다. 
- Random
	- evict 하는 page를 random하게 고름
	- 어떤 경우에는 optimal만큼 성능이 나오지만 운에 따라 달라짐
- Using History
	- 과거에 사용되었던 page정보를 이용해서 evict할 page를 정하자
	- Least Recently Used
	- Least Frequently Used
- LRU
	- 80-20 workload (80%의 reference가 20%의 page로 가는 경우): 이때는 FIFO 등 보다 OPT에 더 가까운 성능이 나온다. 
	- looping Sequential workload (특정 개수의 page를 순차적, loop로 접근하는 경우): LRU는 Cache size가 접근하는 page개수보다 작으면 hit rate가 0이 되어서 성능이 가장 안좋다.
	- Implementation
		- 모든 memory reference보다 해당 내용을 저장해야 한다. 
		- Counter로 구현: 매 접근시 모든 table의 clock을 올려야 해서 시간+공간을 사용함
		- Stack으로 구현: Hash + Stack을 이용해서 가장 최근 사용된 page가 위에 있게 만듦. 이 구조를 OS가 유지해야 하기 때문에 모든 memory reference마다 Trap해야해서 시간이 오래 걸림
		- Approximating LRU
			- 딱 하나의 bit만을 유지해서 LRU를 approximate한 알고리즘을 만듦
			- 해당 bit의 default 값은 0이고, 추가로 접근시 1로 세팅(by HW)
			- 모든 Page list를 circular로 돌면서 
			- 추가 bit 값이 0이면 해당 page를 evict한다. 
			- 1이면 값을 0으로 만들고 다음 page로 넘어감

Considering Dirty Pages
- dirty bit이 1이다 -> evict할 때 write back해야해서 훨씬 큰 cost가 사용된다 
- 앞에서 돌아가는 프로그램 -> dirty가 아닌 clean을 빼주는 것이 좋다. 
- background 프로그램 -> dirty를 빼주는 것이 좋다 (dirty비율을 낮춰주어서)
- Clustering / Grouping
	- write back이 필요한 page들을 묶어서 evict하는 것
	- 보통 16K정도의 크기까지는 한번에 evict하는 것과 하나만 evict하는게 시간이 비슷하기 때문에 모아서 evict하는게 효율적이다. 

Prefetching
- 앞으로 필요할 것으로 예상되는 Page를 미리 memory로 가져오는 것
- 부팅할 때 등의 특별한 경우 잘 작동하지만 penalty가 크기 때문에 항상 보수적으로 해야한다. 

Thrashing
- working set > physical memory size인 경우 page를 swapping하는데 시간이 많이 걸려서 CPU의 사용량이 줄어든다. 
