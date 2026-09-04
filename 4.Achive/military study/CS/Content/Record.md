
- record와 class의 차이점
	- 두 인스턴스의 비교
		- class에서 object의 Equal을 사용해서 두 인스턴스를 비교하면 두 객체가 같은 object를 가르킬 때만 true가 된다.
		- 그러나 record에서는 두 인스턴스의 내부 필드와 프로퍼티의 값을 비교해서 같은 경우 true가 된다.
	- 새로운 인스턴스를 기존 인스턴스에서 만들 때
		- class의 경우 얕은 복사가 이루어지지만 record의 경우 깊은 복사가 이루어진다. 
		- record를 만들 때 ```new_record = old_record with {~~~}``` 로 작성하여 기존 인스턴스에서 일부만 변형시킨 인스턴스를 만들 수 있다. 

- 