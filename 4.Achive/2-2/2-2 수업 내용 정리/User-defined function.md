User-defined function은 return type을 기준으로 3가지로 분류한다.
- Scalar functions
- Aggregate functions
- Table functions

그러나 우리가 사용할 SQLite에서는 User-defined function을 지원하지 않는다 XD.

+) SQL은 decarative language이기 때문에 우리는 "어떻게 할 것인가"가 아닌 "어떤 것을 할 것인가"를 지시해야 한다. 그리고 같은 작업에서 발생할 수 있는 time complexity의 차이는 compiler가 알아서 처리해줄 것이다. 