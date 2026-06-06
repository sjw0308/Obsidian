
- C++과 같은 것들
	- if, else, else if, for, while, do-while, break, continue, 

- switch
	- C++의 형태로도 사용 가능
	- 추가적인 형태가 있음
	- ![[Pasted image 20260510160225.png]]

- foreach (~)
	- ~에 변수타입 + 파이썬에서 쓰는 반복문 넣어주면 됨

- goto
	- goto (레이블): - (해당 레이블로 이동)
	- (레이블): 

- 패턴 매칭
	- 선언 패턴
		- (변수1) is (Type) (변수2)
		- 변수1이 Type이면 식 전체가 true를 반환하고, 변수2에 변수1을 대입
	- 형식 패턴
		- (변수) is (Type)
	- 상수 패턴
		- (변수) is (상수)
	- (변수) is (조건1 and 조건2)
	- (tuple) is (조건1, 조건2, ...) 
		- tuple의 각 요소와 해당 위치의 조건을 비교
	- 목록 패턴
		- (array) is \[조건1, 조건2, .. , \_]
		- 각 요소들이 해당 조건들을 만족하는지 확인