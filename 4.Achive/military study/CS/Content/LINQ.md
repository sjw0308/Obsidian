
- ``` using System.Linq ```
- 기본문법들
  ``` c#
    var var_name = from tmp_name in 원본_데이터
				    where 어떤 객체들을 고를지에 대한 조건(tmp_name을 포함해서)
				    orderby 해당 객체들을 어떻게 정렬할지
				    select 객체의 어떤 부분을 모을지(무명형식 사용 가능);
  ```
	-  from절은 중첩이 가능함
- Group by
	- ``` group A by B into C ```
	- 위라면 A는 객체, B는 조건, C는 이후에 사용할 그룹변수임
	- C.Key에 B조건이 True인지 False인지가 들어가고, C에는 C.Key에 따라서 True인지 False인지 나뉜 객체들이 들어가게 된다. 
- Join
	- 내부 join
		- 두 데이터들 사이에서 일치하는 데이터들만 반환하는 join
		- ```c#
		  from a in A
		  join b in B on a.XX equals b.YY
		  ```
	- 외부 join
		- 일치하지 않더라도 기준이 되는 데이터의 모든 데이터를 결과에 포함시기는 join임
		- ```c#
		  from a in A
		  join b in B on a.XX equals b.YY into c
		  from b in c.DefaultIfEmpty(new B(){YY이외에 A에 없는거 기본값})
		  ```