
OS가 process에게 편의성을 위해서 매우 넓은 크기의 virtual space를 제공한다
그렇기에 여러 process가 돌아가는 환경에서 physical memory가 부족하게 된다

memory hierarchy에서 memory아래에 있는 HDD, SSD를 main memory처럼 쓸 수 있게 하자! 

Swap Space
- HDD또는 SSD상에서 Main memory와 swap가능한 영역을 swap space라고 한다. 
- Memory -> Disk: Swap out, Disk -> Memory: Swap in
- Swap은 page단위로 일어난다. (page size = swap block size = (일반적으로)4KB)
- Page table 변경점
	- present 또는 valid로 불리는 추가적인 bit을 두고, 해당 bit을 통해서 page가 memory에 존재하는지 아닌지 표현한다. 
	- Page Hit: Page가 memory에 존재함
	- Page Fault: Page가 Disk에 존재함
- Page Fault가 발생시 Memory에 있는 page와 fault가 발생한 page를 replace한다. 
- 이후 해당 instruction을 다시 실행한다. 
- 이 과정은 OS가 handling한다. 

Page Replacement
- Disk가 매우 느리다 = replacement의 cost가 매우 크다 = replacement policy가 매우 중요하다.
- Effective Access Time (EAT)
	- memory 접근시 200ns, page-fault시 8000000ns(=8ms)가 걸린다고 가정하면
	- $$ \text{EAT} = (1-p)\times \text{memory access} + p\times (\text{page-fault time}) \\ =(1-p)\times 200+p\times 8000000 \\=200+7999800\times p\ (\text{ns})$$
	- p: memory reference대비 page fault확률
	- e.g. 1000번에 한번씩 page fault가 발생한다면 EAT = 8.2 (us) -> 평균적으로 41배 느려짐!
	- e.g. 20%이상 느려지지 않게 하기위해서는 $$220>200+7999800p\\0.0000025>p$$ 이다.
- Working set
	- 프로그램이 active하게 access 중인 virtual pages를 working set이라고 한다. 
	- Watermark: memory의 freespace의 기준 같은 것이다. low watermark로 내려가면 free space를 확보해서 high watermark로 올라가도록 만든다. 
	- 