
- 기본적으로 C++과 비슷함
- C#에서는 모든 Type이 object에서 상속된 것

- decimal: double보다 넓은 범위 표현이 가능한 실수형
- object: 컨테이너 형태, 임의의 형태를 담을 수 있는 자료형
- (Type name)? : Nullable자료형, 값에 Null이 들어갈 수 있음
- var: C++의 auto와 같음

- 공용형식 자료형
	- .NET 언어끼리 공통으로 사용가능한 자료형
	- System.(Type name) 으로 사용가능

- 형식 관련 함수
	- (변수).GetType() - 타입을 반환하는 함수
	- (변수).ToString() - 변수의 데이터를 String으로 변환해주는 함수
	- (Type).Parse(문자열) - 문자열을 해당 Type으로 변형시켜주는 함수. 변형 불가능 하면 오류로 종료
	- (Type).TryParse(문자열, out 변수) - 문자열을 해당 Type으로 변형시켜서 out 에 있는 변수에 저장, Parse와는 다르게 불가능하면 전체가 false가 나옴

- 문자열 관련 함수들 
	- ![[Pasted image 20260510144449.png]]
	- ![[Pasted image 20260510144530.png]]
	- ![[Pasted image 20260510144619.png]]
- 문자열 Formatting
	- {첨자, 맞춤: 서식}
	- 맞춤: 총 몇 칸에 넣을 것인가. 양수면 오른쪽 정렬, 음수면 왼쪽 정렬
	- 첨자: 맞추기 전에 몇 칸을 비워둘 것인가
	- 서식: ![[Pasted image 20260510150128.png]]
		- 이외에도 날짜 등을 서식할 수 있음
	- $를 문자열 앞에 붙이면 {}안에 식이나 변수 명을 적을 수 있음
