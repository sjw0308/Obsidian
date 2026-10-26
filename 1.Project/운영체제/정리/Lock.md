   
Basic Criteria
- Mutual exclusion
	- 한 Thread가 lock을 잡았다 = 다른 thread들이 해당 section으로 들어오지 못한다
- Fairness
	- 모든 thread들이 lock을 fair하게 잡을 수 있어야 한다
- Performance
	- time overheads가 적어야 한다. 

Controlling Interrupt
- 가장 기본적인 구현
- lock을 잡으면 interrupt를 disable시키고 unlock때 다시 enable시킴
- timer interrupt가 발생하지 않아서 context switch가 되지 않음
- user에게 맡기기에는 너무 큰 신뢰가 필요함
- interrupt의 무시는 하나의 CPU에서만 적용되고, multi processor에서는 작동하지 않음

Hardware support
- hardware에서 atomic한 instruction을 제공하지 않으면 여러 문제가 발생한다. 

Test And Set
- ```C
  int TestAndSet(int* ptr, int new){
      int old = *ptr;
      *ptr = new;
      return old;
  }
  ```
- 위 operation을 atomic하게 실행 (by hardware 지원)
- test and set을 이용하여 lock구현 예시
	- ```C
	  void init(lock_t *lock){
	      lock->flag = 0;
	  }
	  void lock(lock_t *lock){
	      while(TestAndSet(&lock->flag,1) == 1) {}
	  }
	  void unlock(lock_t *lock){
	      lock->flag = 0;
	  }
	  ```
	- 아무도 lock을 안잡고 있을 때 -> flag = 0이므로 while문에서 flag=1로 바꾼 후 문한 loop에 걸리지 않고 바로 return되어서 해당 section을 실행. 이후 unlock을 통해서 lock에서 나올 때 flag를 다시 0으로
	- 누군가 lock을 잡고 있을 때 -> flag = 1이므로 while문에서 무한 loop를 돌면서 대기 -> spin lock
Compare And Swap
- ```C
  int CompaeAndSwap(int* ptr, int expected, int new){
      int actual = *ptr;
      if(actual == expected) *ptr = new;
      return actual;
  }
  void lock(lock_t *lock){
      while(CompareAndSwap(&lock->flag, 0, 1) == 1) {}
  }
  ```
- Test And Set과 유사하지만 lock-free synchronization에 더 유리하다. (아직은 뭔지 모름)
Evaluating two locks
- Correctness -> 통과
- Fairness -> 탈락
- Performance -> 탈락

Fetch And Add
- ```C
  int FetchAndAdd(int* ptr){
      int old = *ptr;
      *ptr = old+1;
      return old;
  }
  ```
Ticket Lock
- ```C
  typedef struct __lock_t {
      int ticket;
      int turn;
  } lock_t;
  void lock_init(lock_t *lock){
       lock->ticket = 0;
       lock->turn=0;
  }
  
  void lock(lock_t *lock){
       int myturn = FetchAndAdd(&lock->ticket);
       while(lock->turn != myturn) {}
  }
  void unlock(lock_t *lock){
       FetchAndAdd(&lock->turn);
  }
  ```
- 은행을 생각하면 됨. ticket을 뽑고 turn이 ticket과 같으면 내가 사용, 아직 turn이 안왔으면 대기
- ticket을 뽑은 순서대로 lock을 부여받기 때문에 Fairness문제를 해결했다
- 그러나 아직 Performance문제는 해결하지 못했다. 

Performance를 해결하는 방법들
- Just Yield
	- lock을 못잡으면 도는 while문 안에 yield넣기
	- Context switch가 n-1번 도는 문제는 여전히 남아있다. 
	- starvation 문제가 발생한다. 
- 그러면 어떻게 해결할까? -> yield처럼 runnable로 가는게 아닌 sleep state로 갔다가 lock이 풀리면 runnable이 되는 걸 만들자!
- Park
	- park(): thread를 sleep에 넣음
	- unpark(threadID): 특정 threadID를 깨움
- queue를 이용한 Sleeping instead of Spinning
	- ```C
	  typedef struct __lock_t {int flag; int guard; queue_t *q} lock_t;
	  void lock_init(lock_t *m){
	       m->flag = 0;
	       m->guard = 0;
	       queue_init(m->q);
	  }
	  
	  void lock(lock_t *m){
	       while(TestAndSet(&m->guard, 1)==1) {}
	       if(m->flag==0){ //내가 lock을 잡는 경우
	           m->flag = 1;
	           m->guard = 0;
	       } else { //다른 누군가 lock을 잡고 있는 경우
	           queue_add(m->q, gettid())
	           m->guard = 0;
	           park();
	       }
	  }
	  void unlock(lock_t *m){
	       while(TestAndSet(&m->guard, 1) == 1){}
	       if(queue_empty(m->q)) m->flag = 0;
	       else unpark(queue_remove(m->q));
	       m->guard = 0;
	  }
	  
	  ```
	- guard는 lock을 잡는 도중에 context switching이 일어나는 경우를 막기 위함이라고 생각하면 될듯
	- unlock에서 대기에 다른 thread가 있으면 flag를 0으로 바꾸지 않는 이유는?
		- 내 lock을 unpark한 thread로 이미 넘긴 것이기 때문이다. 
		- unpark된 thread는 lock을 return하는 code부터 실행하게 된다. 
	- 문제들
		- spin-wait을 완전히 없애진 못함. 그러나 확률(시간)을 확실히 줄임
		- 위 코드에서 thread A가 park되기 직전에 스위칭 -> 이때 원래 lock이었던 thread B가 unlock을 실행해서 A를 unpark -> 돌아온 A가 park를 실행하면서 A가 sleep forever!
- Setpark
	- setpark(): setpark가 실행된 이후 park전에 unpark이 불린다면 그 다음 park를 무시한다. 
	- setpark()를 lock에서 park()의 두줄 위(m->guard = 0 바로 위)에 넣으면 sleep forever문제를 해결할 수 있다. 
