
- 일반화 메소드
	- 만들 때 
		- ```한정자 반환형식 메소드이름<형식_매개변수> (매개변수){...}```
	    - ```ex) public void CopyArray<T> (T[] source, T[] target){...}```
	- 쓸 때 
		  - ```CopyArray<int>(source, target);```

- 일반화 클래스
	- ```class Array_Generic<T>{...}```

- 형식 매개변수 제한
	- ``` where 형식_매개변수 : 제한조건 ```
	-  ![[Pasted image 20260528180135.png]]

- 일반화 컬렉션
	- ```List<T>, Queue<T>, Stack<T>, Dictionary<TKey, TValue>```
- foreach를 사용할 수 있는 일반화 클래스
	- IEnumerator GetEnumerator() -> IEnumerator\<T> GetEnumerator()
	- 메소드들은 기존과 같지만 T Current{get;}을 구현해야함