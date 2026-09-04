
- Interface
	- 메소드, 이벤트, 인덱서, 프로퍼티만 가질 수 있고, 이것들은 구현부를 갖지 않음
	- 위 요소들을 public 한정자로 수식해야 함
	- interface를 상속받은 class들은 메소드와 프로퍼티를 구현해야 함

- 다중상속
	- class는 다중상속 불가
	- interface는 가능

- Abstact Class
	- 구현은 가질 수 있지만 인스턴스를 가질 수 없음
- 추상 메소드
	- interface적인 요소 -> 상속 받은 클래스가 구현해야 하는 메소드
	- 모든 메소드는 public, protected, internal, protected internal 중 하나로 수식해야 됨