
TLB(Translation Lookaside Buffer)
- MMU의 일부분으로 존재한다. HW cache로 virtual to physical address들을 저장한다. 
- SRAM이기 때문에 접근이 매우 빠르다
Address Translation
- Without TLB
	1. CPU에서 MMU로 Virtual address(VA)를 넘김
	2. MMU에서 Page table entry address(PTEA)를 계산하여 memory에 있는 Page table에 접근
	3. Memory에서 PTE(Page Table Entry)를 MMU로 보냄
	4. MMU에서 Physical address(PA)를 계산해서 Memory접근
	5. 해당 주소의 Data를 CPU에 전송
	- 2,3에서 Paging사용에 따른 추가적인 memory 접근이 발생함
- With TLB
	1. CPU에서 MMU로 VA를 넘김
	2. MMU에서 VPN를 TLB에서 찾음 (여기서 HIT인지 아닌지에 따라 두가지 경우로 나뉜다)
	- HIT인 경우
		1. TLB에서 MMU로 PTE를 반환함
		2. MMU에서 PA를 계산하여 memory에 접근
		3. memory에서 data를 CPU로 반환
	- Miss인 경우
		1. PTEA로 Memory의 Page Table에 접근
		2. Memory에서 PTE를 반환하여 TLB를 업데이트 하고, MMU에도 넘김
		3. MMU에서 PA를 계산하여 memory에 접근
		4. memory에서 data를 CPU로 반환
		- Miss시 추가적인 memory 접근이 일어남
	- 그렇지만 TLB Miss는 꽤 rare하다 -> Locality 때문
- 복습
	- Locality -> Temporal locality + Spatial locality
TLB Entry
- TLB는 full associative method로 관리된다. 
- TLB에는 VPN, PFN, Other bits들이 저장되어 있고, VPN이 들어오면 HW의 지원으로 모든 entry내부를 parallel하게 search하여 맞는 entry를 찾는다.

TLB Miss는 누가 handling할까?
- HW가 하는 방법 vs SW가 하는 방법
- Hardware-managed TLB
	- 각 process의 page table시작점을 저장하고 있는 register인 Page-table base register(PTBR)이 존재한다. 
	- Miss 발생시
	1. 현재 Page table의 위치를 PTBR을 통해서 알아냄
	2. 정확한 PTE를 찾음
	3. 해당 PTE를 반환함
	4. TLB를 반환받은 PTE를 이용해서 update함
	5. instruction을 다시 실행함
- Software-managed TLB
	- Miss 발생시
	1. HW에서 exception발생
	2. OS의 trap handler가 불림
	3. OS가 Page Table에서 PTE를 찾음
	4. OS가 특정 instruction을 통해서 TLB를 update
	5. Return from the trap
	6. HW에서 instruction을 다시 실행함
- 각각의 장단점
	- HW-managed TLB
		- 장점: 효율적임(trapping으로 인한 overhead가 없음)
		- 단점: CPU design이 복잡해짐, flexibility가 줄어듦
	- SW-managed TLB
		- 장점: CPU design이 쉬움, HW를 변경하지 않고 page table의 구조 등을 변경할 수 있기 때문에 flexibility가 높음
		- 단점: 비교적 느림(trap handler가 실행되어야 하고, 그 동안 해당 process는 멈춰있어야 함)

Context Switch가 일어나면 어떻게 할까?
- 만약 아무런 작업도 해주지 않으면 TLB내부에 같은 VPN을 가지는 entry가 여러개 생길 수 있음
- Simple solution
	- context switch가 일어나면 TLB를 비워버리기
	- 이것도 HW로 할수도 SW로 할수도 있다. 
- Better solution
	- Address Space Identifier(ASID)를 TLB에 추가하여 어떤 작업에서 들어온 것인지 구분하기
