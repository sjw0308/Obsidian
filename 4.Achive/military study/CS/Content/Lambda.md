
- 먼저 대리자를 선언한 후 ```대리자이름 메소드이름 = (매개변수 목록) => 식``` 또는 아래와 같이 여러줄로 선언 가능
- ```C#
  대리자이름 메소드이름 = delegate(매개변수 목록){ //delegate 없애고 => 추가해서 변경 가능
    ~~
    return 결과
  }
  ```
- 이렇게 선언한 후 나중에 메소드를 사용하면 됨

- Func 대리자
	- ``` Func<매개변수 type1, ... ,Type of result> func_name = (매개변수 목록) => {~~~} ```
- Action 대리자
	- ``` Action act_name = () => {~~} 또는 Action<매개변수 type1, ...> act_name = () => {~~} ```
	- return이 없는 Func라고 생각하면 됨

- 식으로 이루어지는 멤버
	- class내부의 맴버를 식으로 만들 수 있음
	- ``` 맴버 => 식  (ex. public void Add(string name) => list.Add(name)) ```
	- 생성자나 종료자, get/set도 이런식으로 만들 수 있음