- Selection
	- $\sigma_C(R)$ 로 표기하고, $R$이라는 table에서 $C$ condition을 만족하는 요소를 select하겠다는 의미이다. 
	- Sql에서는 ``` SELECT *
		FROM (Table name)
		WHERE (Condition) ```
		로 사용할 수 있다. 
- Union
	- $R1 \cup R2$
	- ``` SECLECT * FROM R1
		UNION ALL
		SELECT * FROM R2```
- Set difference
	- $R1 - R2$
	- ```SELECT * FROM R1
		EXCEPT
		SELECT * FROM R2```
- Intersection
	- Derived operator using minus: $R1 \cap R2 = R1 - (R2 - R1)$
	- Derived using join: $R1 \cap R2 = R1 ⋈ R2$ 
- Projection
	- $\pi _{A1, ..., An} (R)$, $A1, ..., An$: Column names, R: Table name
	- ```SELECT (Column names)
		FROM (Table name)```
- Cartesian product
	- $R1 \times R2$
	- Combine each rows(tuples) in $R1$ and each rows(tuples) in $R2$. Traditionally rare in practice because it is very expensive operation
- Equi-join
	- $R1 ⋈_{A=B} R2 = \sigma_{A=B} (R1 \times R2)$ , $A$: $R1$의 attribute, $B$: $R2$의 attribute.
	- ```SELECT *
		FROM R1, R2
		WHERE R1.A = R2.B```
- Theta-join 
	- Join operation for arbitrary condition
	- $R1 ⋈_\theta R2 = \sigma_\theta (R1 \times R2)$
- Natural join
	- Join if all common attribute is equal
	- $R1⋈R2$
- Outer join
	- Natural join + join the rows that did not join with null keyword
	- Left outer join: ⟕, Right outer join: ⟖, Full outer join: ⟗
- Aggregation: GROUP BY
	- Five aggregation function: sum, count, average, maximum, minimum
	- $_A G_F(R)$ , $A$: Attribute name, $F$: Attribute function, $R$: Table name
	- $R$ table에서 $A$마다 $F$를 return 해달라는 의미이다. 
	- ```SELECT A, F
		FROM R
		GROUP BY A```
- Aggregation: GROUP BY ... HAVING
	- 여기서 HAVING 은 SELECTION의 WHRE과 같은 기능을 한다. 그러나 WHERE은 Table의 attribute에 condition을 지정하는 것이고, HAVING은 GROUP BY의 result value에 condition을 지정하는 것이기 때문에 보통 WHERE을 먼저 실행하는 것이 processing할 때 효율적이다.
	- $\sigma_C(_A G_F(R))$ , $C$: Condition 
	-  ```SELECT A, F
		FROM R
		GROUP BY A
		HAVING C```
- ORDER BY
	- Output에 순서를 지정해준다. 
	- ``` ORDER BY F ASC(DESC)``` , F: GROUP BY 에서 사용했던 F, ASC: 오름차순, DESC: 내림차순
