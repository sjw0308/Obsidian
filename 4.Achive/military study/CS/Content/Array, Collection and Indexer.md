
- Array
	- ```type[] var_name = new type[num];```
	- 만들 때 초기화 방법
		- ```type[] var_name = new type[num] {ele1, ele2, ...};```
		- ```type[] var_name = new type[] {ele1, ele2, ...};```
		- ```type[] var_name = {...};```
	- index에 ^1을 넣으면 뒤에서 첫 번째 원소임
	- Array.Function()
		- ![[Pasted image 20260519182414.png|601]]
	- Slice
		- ```type[] sliced = origin[start_idx .. ending_idx + 1];```
		- ps) 위에서 사용한 ^을 이용하면 편함
	- 2D Array
		- ```type[,] var_name = new type[length1, length2];``` 로 생성
		- ```var_name[idx1, idx2]```로 접근
		- ,의 수로 차원을 정할 수 있음
	- 가변 배열
		- ```type[][] var_name = new type[# of arrays][];```로 선언하면 
		- ```var_name[i]```가 가변 배열이 되어서 길이가 임의로 가능함

- Collection
	- ICollection 인터페이스를 상속 받은 C# 클래스들. Array, ArrayList, Queue, Stack, Hashtable이 있음
	- ArrayLIst
		- ``` ArrayList a = new ArrayList(); a.Add(value); a.RemoveAt(idx); a.Insert(idx, value); ```
	- Queue
		- ```Queue q = new Queue(); q.Enqueue(value); type a = q.Dequeue();```
	- Stack
		- ```Stack s = new Stack(); s.Push(value); type a = s.Pop();```
	- HashTable
		- ```HashTable h = new HashTable(); h[key] = value;```
	- 초기화
		- HashTable 아닌 것들
			- 만들 때 ()안에 기존 Array, Stack 등 넣기
			- ArrayList는 Array처럼 직접 선언 가능
		- ```HashTable h = new HashTable(){[key1]=value1, [key2]=value2, ...}```

- Indexer
	- 인덱서는 C++에 있었던 \[] 오버라이드 같은거임
	- ```C#
	  class class_name{
		  한정자 type this(type idx_name){
				get{...}
				set{...}  
			}
	  } 
	  ```
	- 나중에 ```instance_name[idx]```로 접근하게 하는거임

- foreach로 접근 가능한 객체
	- IEnumerable 을 상속 받으면 됨
	- 해당 인터페이스의 메소드는 ```IEnumerator GetEnumerator()``` 하나밖에 없음
		- 구현예시![[Screenshot_20260519_223835_eBook.jpg]]
	- 그럼 IEnumerator는 뭘까?
	-  ![[Screenshot_20260519_224024_eBook.jpg]]를 가지는 인터페이스를 상속 받은 것들임
	- 위 구현예시에서는 이를 yield를 통해서 피해감