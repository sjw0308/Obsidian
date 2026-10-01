
OS는 Hardware와 Task를 잇는 역할을 한다.

- Virtualization
	- OS는 task에게 각각 virtual한 CPU와 Memory가 있는 것처럼 보이게 한다. (virtual storage도 있음)
- Concurrency
	- multi-threaded program에서 공유 중인 virtual memory space에서 concurrency problem이 발생한다. 
	- thread: 하나의 process에서 동시에 여러 작업을 진행하게 해주는 개념으로 같은 가상주소공간을 공유한다. 
- Persistence
	- DRAM은 휘발성이다. 따라서 File system을 이용해서 가상화와 Persistence를 모두 지원한다. 

OS의 Design Goal
- Build up abstraction
- Provide high performance
- Protection between application (Isolation)
- High degree of reliability
- etc