
Paging을 Linear table로 만들었을 때 문제점
- 4KB의 page를 4B의 PTE로 mapping한다고 가정하면 32 bit address space에는 4MB의 page table이 필요하다. (process 하나당)
- 너무 큼!
- 그렇다고 Page의 크기를 키우면 internal fragmentation이 문제가 됨
- 왜 문제가 발생할까?
	- invalid상태로 낭비되는 table공간이 너무 많기 때문이다. 

Hybrid Approach
- Segmentation + Paging 방법이다
- segment 방법에서 했던 code, stack, heap에 각각 Page Table의 base bound를 둔다. 
- Design 방법
	- 주소의 MSB 2-bits를 어떤 segment를 의미하는지 표시하는 용도로 사용한다. 
	- ![[Pasted image 20261121224353.png]]
- 문제점
	- segmentation에서 발생했던 문제가 다시 발생함
	- Page table이 연속적인 구간이 필요하기 때문에 External fragmentation이 발생할 수 있음
	- Heap과 Stack영역이 자라서 Free-space management를 해야되는데 쉽지 않음

Multi-level Page Table
- Page Table을 page-sized units의 크기로 쪼개서 tree의 형태와 비슷하게 만들자
- Page directory로 page table이 valid한지 관리
- Page Directory (2 level e.g.)
	- 기존 PTBR에서 PDBR로 바꿔서 Page directory를 가리키게 함
	- 기존주소의 VPN을 Page directory index와 Page table index로 나눈다. 
	- PDBR의 Page Directory Entry에서 Page table의 Physical frame number를 가져옴
	- Page table로 가서 해당 table의 PTE를 통해서 Data가 있는 PFN을 가져옴
- 장점
	- 메모리를 아낄 수 있음: 배정되지 않은 page의 page table도 배정하지 않음으로써 page table의 메모리를 아낄 수 있음
	- memory management하기 쉬움
- 단점
	- TLB Miss가 나면 memory access 횟수가 level수 만큼 증가한다
	- Complexity: 하나의 테이블을 더 manage 해야됨

Inverted Page Table
- 기존에는 Process별로 Page table을 두어야 해서 여러개의 page table이 필요하다 -> page table의 전체 용량이 커지는 이유이다
- 따라서 process별로 나누지 말고, VPN을 Hash function을 통해서 index를 얻음 -> 이 index를 이용해서 찾아간 table entry에서 VPN, PID를 비교해서 PFN을 얻음
- 장점
	- 하나의 page table만 유지하면 되기 때문에 memory가 적게 사용된다. 
	- TLB miss시 memory reference 횟수가 더 적다
- 단점
	- Hash collision을 resolve할 때 worse case이면 너무 많은 시간이 걸린다. (최대 O(page table size), 이걸 줄이려면 table size가 커져야되는 딜레마)
	- page sharing을 구현하기 어렵다. (이걸 구현하려면 또 page table에 공간이 필요하게 된다)
	- swapping을 지원하기 어렵다. 
