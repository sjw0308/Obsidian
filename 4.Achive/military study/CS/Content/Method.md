
- static method
	- 원래 method는 instance 이름과 함께 불리지만 static method는 class name과 함께 불린다
	- static method 내부에서는 instance member를 참조해서는 안된다.

- 참조자
	- 매개변수의 Type앞에 ref를 적어서 참조자로 매개변수를 받을 수 있다. 
	- 해당 함수를 사용할 때 ref를 매개변수 앞에 붙어서 전달해야한다. 
	- 매개변수 뿐만이 아니라 return결과에도 ref를 붙일 수 있다. 붙이면 return에 넣어둔 class변수를 외부에서 변경이 가능하다. 
- 출력 전용 참조자
	- 2개 이상의 값을 반환하기 위해서 매개변수에 참조자를 사용하는 경우에는 미리 값을 선언할 필요 없다. 이 경우에는 ref가 아닌 out을 붙여서 출력 전용 매개변수를 만들 수 있다. 

- 가변개수 인수
	- functions (params (Type)[] args)
- default 인수
	- 인수 = ~로 선언하면 default값을 넣을 수 있음
- local 함수
	- method안에서 선언되서 거기서만 사용되는 함수