
- 생성자
	- class 이름하고 같게 
	- this 생성자 - 생성자:this(~)를 붙이면 ~가 인자인 생성자를 먼저 부르고 나머지 생성자에 대한 내용을 수행함
- 종료자
	- ~class 이름

- 접근 한정자
	- ![[Screenshot_20260511_222441_eBook.jpg]]

- as
	- ex Cat cat = mammal as Cat
	- 이런식으로 쓰면 mammal이 Cat이면 cat에 mammal을 복사하고, 아니면 cat에 null을 대입

- 오버라이딩
	- 부모 class에서 메소드를 선언할 때 앞에 virtual을 붙임
	- 자식 class에서 해당 메소드를 오버라이딩할 때 override를 붙임
	- 부모 class에서 메소드에 sealed를 붙이면 자식 class에서 override 불가
- 메소드 숨기기
	- 부모 class의 메소드와 같은 이름의 메소드를 자식 class에서 만들 때 new키워드를 넣으면 자식 class에서는 부모 class의 메소드가 가려짐

- readonly
	- class의 필드를 만들 때 readonly를 붙이면 해당 필드는 생성자에서만 초기화가 가능함

- 중첩 class
	- class내부에 class를 선언하는 것
	- 외부 class에 접근할 때는 outer키워드를 사용하면 되고 외부 class의 private member에 접근이 가능함

- 확장 메소드
	- 기존 class의 기능을 확장하는 방법
	- ![[Pasted image 20260513231135.png]]
	- ![[Pasted image 20260513231206.png]]
	- ![[Pasted image 20260513231240.png]]

- 구조체
	- ![[Pasted image 20260513232729.png]]
	- readonly 구조체를 만들 수 있고, 이때 내부 필드와 메소드는 모두 readonly로 만들어야됨

- 튜플
	- 파이썬의 튜플과 비슷함
	- var를 이용해서 선언하면 편함
	- ![[Pasted image 20260513233234.png]]