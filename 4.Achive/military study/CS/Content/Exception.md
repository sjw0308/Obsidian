
- try ~ catch
	- try안에서 throw함수를 이용해서 예외를 던질 수 있음
		- ``` throw new Exception("~~~");```
	- 던진 예외는 catch에서 받는데 catch의 매개변수는 System.Exception클래스에 있는 거임
	- Exception e 라면 e.Message로 예외 메세지를 확인 가능함

- finally
	- finally구문은 try ~ catch구문을 실행 후 마지막으로 실행할 코드를 두면 됨

- Exception class
	- Exception을 상속받는 class를 만들면 됨
	- 그 후 catch에서 해당 class를 받고 catch 함수를 쓸 때 when을 사용해서 특정 상황에 대한 예외를 잡을 수 있음