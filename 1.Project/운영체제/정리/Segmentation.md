
Segmentation
- Segmentation은 사용하는 memory공간을 크게 code, stack, heap으로 나눠서 각자의 base, bound register를 나눠서 저장한다. 
- stack은 위로, heap은 아래로 자라고, code는 자라지 않기 때문에 physical memory에서 stack-code-heap순으로 배치에서 초기에 낭비하는 memory를 최소화 한다. 
- Address Translation on Segmentation
	- Explicit approach: virtual address의 MSB 2bits를 해당 주소가 어떤 segment를 사용할 것인지를 나타내게 한다. (00=code, 01=heap, 10=stack)
	- ![[Pasted image 20261115202151.png|486]]
- Stack segment
	- stack은 code나 heap과 다르게 위로 자란다. 그렇기에 시작 주소를 stack부분의 가장 아래로 잡고, 거기서 address를 빼서 address를 구해야한다. 
	- 그래서 base, bound register뿐만이 아니라 direction에 대응되는 bit을 두어서 code, heap은 1로, stack은 0으로 만들어야 한다. 
- Support for Sharing
	- Code는 read와 execute는 지원해야하고, write는 막아야 한다. 이와 다르게 heap과 stack은 read, write는 지원해야 하지만 execution은 막아야 한다. 
	- e.g. Code sharing: 두 process가 같은 code를 사용하면 code segment를 공유하여 사용할 수 있다. 
	- 그래서 protection bit을 만들어서 점검한다
	- ![[Pasted image 20261115205821.png|496]]
- Fragmentation
	- External fragmentation: physical memory상에서 free된 공간이 나눠져 있어서 지금 할당하려는 공간보다 더 많은 free space가 있지만 할당을 못할 수도 있다.
	- Compaction: physical memory의 segment들을 rearranging하는 것. 그러나 compaction을 하려면 process를 멈추고 data를 copy하고 segment register를 바꿔야 한다. 그렇기에 비싼 작업이다. 또한 allocated끼리 뭉쳐놔서 heap과 stack이 더이상 못 자랄수도 있다. 
