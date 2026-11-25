
Concept
- address space를 고정된 크기의 page unit으로 나눈다
- Physical memory도 같은 크기로 나누고, 이를 page frame이라고 한다.
- 중간에 page table을 두어서 virtual address와 physical address를 mapping한다. 
장점
- Flexibility: virtual address space의 추상화를 효율적으로 지원한다. (heap, stack이 커지는 것을 고려할 필요가 없다)
- Simplicity: External fragmentation이 없기 때문에 free-space management가 쉽다. 

Address Translation
- Virtual address
	- VPN과 VPO의 두 부분으로 나누어져 있다. 
	- VPN(Virtual Page Number): 몇번째 virtual page를 사용할 것인가
	- VPO(Virtual Page Offset aka Offset): 해당 page의 몇번째 byte에 접근할 것인가
- 위 주소를 Physical address로 바꾸려면 VPN만 PFN(Physical Frame Number)로 변환하면 된다. (VPO는 유지)

Page Table
- VPN과 PFN을 mapping해둔 table이다. 각 table의 요소들을 PTE(Page Table Entry)라고 한다. 
- Page table을 이용해서 VPN을 PFN으로 바꾸고 이전 VPO를 그대로 이용하면 Physical address를 만들 수 있다. 
- Page Table은 어디에 저장할까?
	- Page table은 매우 커질 수 있다. 따라서 register에는 저장할 수 없고 main memory에 저장하게 된다. 
- Common Flags of PTE
	- Valid bit: 실제로 mapping이 되어 있는지를 나타낸다
	- Protection bit: Read-Execution인지 Read-Write인지를 나타낸다. 
	- 이외에도 Swapping 등에 사용되는 bit들이 같이 저장된다. 
- Page Sharing
	- A에서 B를 fork()를 실행하면 우선 모든 Page들을 공유한다. 
	- 이후 B에서 write를 실행하면 A의 page를 그대로 복사하고 이후 해당 B를 사용한다. (이때도 code 등은 공유하고 있음)
- 문제점
	- Internal fragmentation
		- 한 Page의 일부만 사용 중이고 남는 부분이 존재할 수 있음
		- Solution: 더 작은 page size를 사용해야 함
	- Performance degradation
		- Page Table접근을 위해서 memory접근이 늘어남
		- Solution: TLB(Translation Lookaside buffer)를 사용해서 해결할 수 있음
	- Huge page tables
		- Page table이 너무 커질 수 있음
		- Solution: Page의 크기를 늘리고 싶은데 이러면 internal fragmentation문제가 심각해짐. 따라서 hybrid, multi-level paging을 이용해서 해결한다
