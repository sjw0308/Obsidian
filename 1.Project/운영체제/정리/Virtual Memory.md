
process들은 flat memory(address가 0번부터 시작하는 memory)를 가정하고 돌아간다. 

RAM (Random access memory)
- SRAM: 하나의 cell을 만드는데 4~6개의 trasister가 사용되어서 비싸지만 빠른 RAM
- DRAM: 하나의 cell을 만드는데 하나의 trasister와 하나의 capacitor만 필요하여 싸지만 비교적 느린 RAM
- RAM은 휘발성 메모리이기 때문에 SSD, HDD등 비휘발성 메모리를 같이 사용한다. 
Memory Hierarchy
- 컴퓨터는 작고 빠른 L0(register)에서부터 크지만 느린 SSD, HDD까지 구조를 갖는다
- 여기서 목표는 속도는 SRAM수준으로, 용량은 SSD, HDD로 느끼게 만드는 것을 묙표로 한다. 
- 이런 목표의 달성을 돕는 것은 Locality 특성이다. 
	- Temporal locality: 같은 데이터에 시간차이를 두고 다시 접근하는 것
	- Spatial locality: 바로 근처의 데이터에 이어서 접근하는 것
- e.g.
	- ```C
	  sum=0; 
	  for(i=0; i<n; i++)
	      sum += a[i];
	  return sum;
	  ```
	- Data 접근
		- a의 원소에 순서대로 접근 -> spatial locality
		- sum, i에 여러번 다시 접근 -> temporal locality
	- Instruction 접근
		- code를 순서대로 실행 -> spatial locality
		- loop 안의 코드를 여러번 실행 -> temporal locality

Physical Memory
- Virtualization 없이 직접적으로 physical memory에 접근하면.. 
	- 다른 프로세스의 메모리에 접근이 가능해져서 protection의 문제가 발생할 수 있다. 
Virtual Memory
- Virtual address space
	- 각 process들은 자신만의 private한 virtual memory space를 갖는다. 
	- 프로그램은 해당 memory가 virtualized되어있다는 것을 알 수 없다. 
	- process는 다른 process의 memory space에 접근할 수 없다.
- Address translation
	- Physical memory는 OS에서 관리하고 있기 때문에 process에서 virtual memory address를 주면 OS에서 이를 physical address로 젼환하여 메모리에 접근한다. 
- 장점
	- Memory efficiency: 여러 프로그램을 동시에 같은 physical memory에서 실행할 수 있다. 
	- Protection: OS가 process의 isolation을 보장한다. 
	- Ease of programming: 모든 프로그램이 uniform linear address space를 갖기 때문에 관리 및 활용할 때 용의하다. 

Virtual address space
- Address space
	- Linear address space: 0과 양의 정수로 address가 contiguous하게 정의된 space
	- Virtual address space: $N=2^n$ 
	- Physical address space: $M=2^m$
- virtual address space에는 대표적으로 code, heap, stack부분이 존재한다. 