
Definition
- ```sem_init(&s,0,1)``` 0은 같은 process내의 thread끼리 공유한다는 의미이고, 1은 initial value이다. 
- ```C
  int sem_wait(sem_t *s){
    decrement the value of semaphore s by one
    wait if value of semaphore s is negative
  }
  ```
- ```C
  int sem_post(sem_t *s){
    increment the value of semaphore s by one
    if there are one or more threads waiting, wake one
  }
  ```
- sem_wait은 lock과 sem_post는 unlock과 비슷한데 semaphore는 초기에 s 값을 지정하므로써 몇개의 thread를 보낼지 등을 설정할 수 있는 lock의 일반화 버전으로 생각하면 된다. 
- Binary Semaphore = Lock
	- 초기 값이 1이면 lock을 만들 수 있다. 
- s값이 음수일 때 |s|값은 해당 semaphore를 기다리는 thread의 수

Semaphore Applications
- Condition Variables
	- e.g. 부모 thread가 자식 thread를 기다리는 경우 
		1. 부모 thread가 초기값 0으로 sem_init
		2. 부모가 자식 thread를 만들고, sem_wait
		3. 자식 thread가 실행 후 sem_post를 불러서 sleep중인 부모 thread를 깨움
		4. 부모 thread의 남은 부분을 실행
	- lock과 다르게 자식 thread를 만들고 sem_wait을 수행하기 전에 context switch가되어도 상관 없음
- Producer/Consumer Problem
	- Producer $\stackrel{put}{\longrightarrow}$ Buffer $\stackrel{get}{\longrightarrow}$ Consumer 의 구조가 있을 때 Buffer가 한정되어서 producer가 기다리는 경우를 Semaphore를 써서 해결할 수 있다. 
	- ```C
		sem_t empty;
		sem_t full;
		sem_t mutex;
		void *producer(void *arg){
			int i;
			for(i=0; i<loops; i++){
				sem_wiat(&empty);
				sem_wait(&mutex);
				put(i);
				sem_post(&mutex);
				sem_post(&full);
			}
		}
		void *consumer(void *arg){
			int i;
			for(i=0; i<loops; i++){
				sem_wait(&full);
				sem_wait(&mutex);
				int tmp = get();
				sem_post(&mutex);
				sem_post(&empty);
			}
		}
		int main(int argc, char *argv[]){
			// ..
			sem_init(&empty, 0, MAX);
			sem_init(&full, 0, 0);
			sem_init(&mutex, 0, 1);
			// ..
		}
	  ```
	- empty는 buffer에서 비어있는 슬롯 개수, full은 buffer에서 차있는 슬롯 개수, mutex는 critical section을 보호하기 위한 lock의 역할을 한다. 
- Reader-Writer locks
	- writer(insert): 무언가 변화를 주는 작업
	- reader(lookup): 변화는 주지 않고, 보기만 하는 작업
	- reader는 서로 몇명이 같이 lock을 잡아도 되지만 writer는 다른 reader나 writer가 같이 lock을 잡으면 안된다. 
	- ```C
		void rwlock_acuire_readlock(rwlock_t *lock) {
			sem_wait(&rw->lock);
			rw->readers++;
			if(rw->readers == 1) {
				sem_wait(&rw->write_lock); //내가 최초의 reader
			}
			sem_post(&rw->lock);
		}
		void rwlock_release_readlock(rwlock_t *lock) {
			sem_wait(&rw->lock);
			rw->readers--;
			if(rw->readers == 0) {
				sem_post(&rw->write_lock); //내가 마지막 reader
			}
			sem_post(&rw->lock);
		}
		
		void rwlock_acuire_writelock(rwlock_t *lock) {
			sem_wait(&rw->write_lock);
		}
		void rwlock_release_writelock(rwlock_t *lock) {
			sem_post(&rw->write_lock);
		}
	  ```
	- 그러나 reader와 writer간의 fairness문제(starve writer문제)가 있다. 
- Dining Philosophers
	- 철학자 5명이 원형으로 앉아 있고, 5개의 포크가 각 철학자 사이에 있다. 철학자는 중간중간 양쪽의 포크를 사용해서 밥을 먹고 다먹으면 내려둔다. 
	- 우리가 원하는 것: no deadlock, no philosopher starve, concurrency should be high
	- 기본적인 생각
		- 모든 철학자가 일어나서 왼쪽 포크부터 집으면?
		- deadlock발생 (모든 철학자가 왼쪽을 집은 상태로 멈춤)
	- 해결법: 마지막 철학자만 포크를 오른쪽부터 집게 만듦
