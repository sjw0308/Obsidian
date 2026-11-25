
이 강의에서는 Segmentation이 아닌 heap을 management 할 때로 가정한다. 

Assumptions
- memory interface는 malloc(size_t size), free(void* ptr) 로 한정한다.
- No compaction
- 연속된 byte 공간에 allocate됨
- heap size가 고정되어 있음

Splitting
- malloc 할 때 ...
- free chunk들 중에서 배정할 chunk를 특정 policy에 따라서 결정함
- 해당 chunk를 malloc할 부분을 때고 남은 free부분으로 free list node를 수정하는 것을 의미한다. 
Coalescing
- free 할 때...
- 사용중이던 chunk를 free list에 추가함
- 기존 free였던 다른 chunk들과 결합시킬 수 있다면 그 chunk들과 결합하고, free list를 수정한다 
Header
- 우리는 free할 때 size정보 없이 pointer정보만 넘긴다. 그렇다면 어떻게 크기를 알 수 있을까?
- 이전 malloc시 header에 정보들을 넣어둔다. 
- size: malloc한 크기를 저장한다
- magic: unique한 숫자로 allocator가 잘 작동중인지 기록하기 위해서 이용한다. free시 해당부분이 정해둔 숫자와 다르면 에러메세지를 출력한다. 
- ![[Pasted image 20261115232957.png|468]]

free list를 만드는 방법
1. Create and initialize a linked list
2. Create a heap by borrowing memory from the OS kernel (by using mmap system call)
3. Assign one free chunk to the linked list
4. Manage the list whenever malloc or free is invoked
5. Increate the heap size (via sbrk system call) if necessary
- free list의 node는 size, next정보가 들어있다. 
- 시작 할 때 mmap으로 빌리고 싶은 메모리 크기 등을 입력해서 head node를 생성하고, size를 빌린 memory size - node size로, next는 NULL로 설정한다. 
- ![[Pasted image 20261116000146.png|591]]
- 이후 malloc을 하면 header + malloc으로 요구한 크기 만큼의 부분을 split+allocate하고 free chunk의 크기를 그만큼 줄인다. 

Managing Free Space (어떤 free chunk에 malloc 할거냐)
- Best Fit
	- request가 왔을 때 가능한 free chunk 중 가장 작은 것을 선택
	- full search를 해야하기 때문에 찾는데 오래 걸린다. 
	- malloc하고 남은 부분이 작기 때문에 그 부분을 앞으로 사용하지 못할 가능성이 높다. 
- Worst Fit
	- 가능한 것들 중 가장 큰 것을 선택한다. 
	- full search를 해야하기 때문에 찾는데 오래 걸린다. 
- First Fit
	- 가능한 것들 중 가장 처음 찾은 chunk를 사용
	- 앞쪽 메모리 공간만을 많이 사용하게 될 수 있다
- Next Fit
	- 이전에 return 된 (free 된) chunk 다음부터 찾기 시작해서 First Fit을 적용한다.
