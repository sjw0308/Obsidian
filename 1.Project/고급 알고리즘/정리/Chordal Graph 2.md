
Polytime Algorithm for computing a PEO
- for i=1 to n,
     Find a simplicial vertex v, and let $\sigma(v)=i$
     Remove v from the current graph
  $\sigma(\cdot)$ 값을 기준으로 정렬
- Running time? $O(n^4)$ with a naive implementation
- 더 좋은 방법은 없을까? 
	- LexcoBFS를 이용하면 된다

Lexicographical BFS
- 임의의 vertex $v_{0}$ 를 고름
  For i=1 to n-1,
     For each vertex $v\in V-{v_{0},\dots, v_{i-1}}$, $l(v)=a_{0}\dots a_{i-1}$값을 아래와 같이 정의한다
         $a_{j}\in\{0,1\}\ s.t.\ a_{j}=0\ iff\ (v_{j},v)\in E$
     $v_{i}$를 $V-\{v_{0}, \dots , v_{i-1}\}$ 중 $l$값이 가장 큰 vertex로 정한다
  이 순서를 reverse하면 PEO가 됨
- 두 개의 vertices $v_{i}, v_{j}$에 대해서 $d(v_{0}, v_{i})<d(V_{0}, v_{j})$이면 i<j이므로 BFS가 맞다
- Thm. The reverse of lex-ordering is a perfect elimination ordering if G is chordal
	- Pf>
	- V를 reverse of lex-ordering이라고 하자. 그리고 두 개의 vertices v, w에 대해서 $v\prec w$이면 V ordering에서 v가 w보다 앞에 있는 것이라고 하자
	- V가 PEO가 아니라고 가정하면 아래 두개의 조건에 맞는 vertices 3개가 존재한다
		- $v_{1}\prec v_{2}\prec v_{3}$
		- $v_{1}v_{2}\in E,\ v_{1}v_{3}\in E,\ v_{2}v_{3}\notin E$
	- 그러면 새로운 vertex $v_{4}$에 대해서 $v_{2}v_{4}\in E,\ v_{1}v_{4}\notin E$라면 G가 chordal이기 때문에 $v_{3}v_{4}\notin E$이다. 또한 $v_{1}\prec v_{2}\prec v_{3}\prec v_{4}$이다 
	- 이제 다음 작업을 생각하자
		- 아래의 작업으로 만들어질 sequence $v_{1}, \dots, v_{i}$까지 이미 있다고 가정하고
		- $v_{i+1}v_{i-2}\notin E, v_{i+1}v_{i-1}\in E, v_{i}\prec v_{i+1}$ 인 vertex $v_{i+1}$ 중에서 reverse lex-ordering이 가장 큰 vertex에 대해서
			- i-1번을 제외한 sequence에 있는 vertex 중 어떤 것과 adjacent하다면 그 놈을 x라고 함(adjacent한 놈 처음 등장) 
			- 아니라면 sequence에 해당 vertex를 추가하고 이 searching을 반복
	- 이렇게 sequence $v_{1}, \dots, v_{j}$ 와 x를 얻었음
	- 이때 x가 $v_{j-3}$과는 adjacent가 아니라는 것을 증명할꺼임
		- 만약 x가 $v_{j-3}$와 adjacent라면
		- $v_{j-3}\prec v_{j-2}\prec x$를 생각했을 때, $x\prec z$이고 $zv_{j-2}\in E,zv_{j-3}\notin E$인 z가 존재하게 됨 (LexBFS 성질).
		- z가 path들 중에서 j-2번을 제외한 어떤 것과 adjacent라면, $v_{j}$를 선택하는 단계에서 z가 또다른 x로 선택되었어야 됨 -> 모순
		- z가 path들 중에서 어떤 것과도 non-adjacent라면, $v_{j}$를 선택하는 단계에서 같은 조건인데 reverse lex-order가 더 큰 z가 $v_{j}$대신 선택되었어야함 -> 모순
	- 즉 x는 $v_{k},\ k\leq j-4\ or\ k=j$인 vertices와 인접하고 있음. 이때 induced path에서 $v_{j-1}$과 가장 가까운 $v_{k}$를 y라고 하자
	- 그러면 $x, v_{j-1}, \dots, y, x$로 이루어지는 induced cycle의 길이가 최소 4 이상이므로 G가 chordal임에 모순됨. QED