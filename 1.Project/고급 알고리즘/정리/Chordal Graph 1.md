
Coloring problem
- The chromatic number $\chi(G)$ = minimum value k s.t. G admits a k-coloring
- clique of size k (complete graph with k vertices) -> $\chi(G) = k$
	- $\chi(G)\geq \omega(G)$ ($\omega(G)$는 G가 가진 largest clique)임을 의미한다
Coloring Problem for Interval Graph
- Interval graph는 여러 interval들이 존재할 때 각 interval을 하나의 vertex로 생각하고 interval이 겹치면 연결되었다고 간주하는 그래프임
- Lemma. For interval graph G=(V,E), we have $\chi(G) = \omega(G)$
	- Pf>
	- interval의 시작점을 기준으로 G의 interval을 정렬한 것을 $\mathcal{F}$라고 하고 하나의 interval I에 대해서 해당 interval까지 사용한 color set을 $\mathcal{C} = \{1,2,\dots,k'\}$ 라고 하자
	- I의 앞에 있는데 I와 연결된 interval들(+ I)이 clique를 이룬다
	- 그렇기 때문에 해당 연결된 interval들의 수가 k'미만이면 이전에 있던 색으로 색칠가능하지만 k'개라면 k'+1번째 색이 필요하다. 
	- 따라서 k'이 k'+1로 올라가는 것은 clique의 크기가 늘어날 때 뿐이다. 

Perfect Graph
- 정의: A perfect graph G=(V,E) is a graph in which the chromatic number of every induced subgraph equals the size of a largest clique of that subgraph. 
	- G의 모든 subgraph S에 대해서 $\chi(S) = \omega(S)$인 그래프
- perfect graph에서는 coloring problem과 maximum clique problem, vertex cover problem을 poly-time에서 풀수 있다

Perfect Elimination Ordering
- G의 한 vertex v가 **simplicial**이다 = $\{v\} \cup N(v)$가 complete이다.
- 정의: G의 **perfect elimination ordering**은 vertex의 어떤 나열 $\{v_{1},\dots,v_{n}\}$에서 $v_{i}$가 G의 subgraph인 $\{v_{i}, \dots, v_{n}\}$의 simplicial일 때 해당 나열을 의미한다 (아까 풀었던 interval graph 풀이에서 만들었던 순서의 역순)
- perfect elimination ordering of G가 주어지면 chromatic number와 corresponding coloring 을 linear time으로 이길 수 있다

Chordal Graph
- 정의: A graph G = (V,E) 가 chordal이다 = G의 모든 induced cycle이 triangle이다. 
- Thm. A graph G is chordal $\iff$ it has a perfect elimination ordering
	- pf ($\Leftarrow$)
		- G가 길이가 4인 induced cycle을 갖는다고 가정하자
		- 그 cycle 중에서 elimination order가 가장 작은 vertex를 v1이라고 하면 나머지 v2~v4와 연결되어 있으므로 v2~v4가 clique(triangle)을 이룸 -> 모순
	- pf ($\Rightarrow$)
		- Claim 1. Every chordal graph has a simplicial vertex
		- Claim 1을 이용해서 G에서 simplicial vertex를 v라고 할 때 G-v가 chordal이므로 induction으로 증명을 할 수 있다.
사전작업
- 정의: 모든 non-adjacent vertex x,y에 대해서 어떤 set S가 G-S에서 x,y가 disconnect로 만든다면 S를 x-y separator라고 한다
- Lemma. A chordal graph has two non-adjacent simplicial vertices, or it is clique
	- 이 Lemma가 Claim 1보다 더 강함
	- pf>
		- Claim 2: For any two non-adjacent vertices x, y of G, its minimal x-y separator is a clique
		- Claim 2가 성립한다고 가정하면
		- vertex의 수에 대해서 induction을 할 것이다
		- G가 clique이면 증명끝이므로 아니라고 가정하자
		- x, y를 non-adjacent vertices라고 하고, S를 minimal x-y separator라고 하자. 또한 X를 G-S에서 x를 포함하는 component라고 하자
		- 이제 G의 subgraph인 $X\cup S$를 생각하자
			- 이게 clique라면 x는 simplicial vertex라는 것이다
			- 아니라면 induction에 의해서 이 subgraph에는 두개의 non-adjacent simplicial vertices가 있는데 이때 S가 clique이므로 적어도 한 vertex는 V(X)에 포함되어 있다. 
		- 즉 어떤 경우에도 V(X)에 적어도 하나의 simplicial vertex가 있다. 
		- y에 대해서 같은 방법을 취하면 X, Y에 각각 simplicial vertex가 있는데, X, Y는 서로 다른 component이기 때문에(x, y가 non-adjacent라서) 구한 두 simplicial vertices는 non-adjacent이다. 
- Claim 2. For any two non-adjacent vertices x, y of G, its minimal x-y separator is a clique
	- S를 minimal x-y separator라고 하고, u, v를 S에 있는데 non-adjacent인 두 vertices라고 하자. 또한 해당 separator에 의해 나뉜 component 중 x를 포함하는 것을 X, y를 포함하는 것을 Y라고 하자
	- 그러면 u, v는 각각 X, Y에 모두 연결되어 있어야 한다. 연결되어 있지 않다면 해당 vertex를 제외해도 separator가 되기 때문에 S가 minimal이라는 조건에 모순된다. 
	- X, Y가 connected component이기 때문에 u, v를 포함하는 cycle이 생기게 되고, uv edge가 없기 때문에 해당 cycle은 적어도 길이가 4인 induced cycle을 갖는다. 
	- 이는 G가 Chordal이라는 조건에 모순이 되기 때문에 증명 끝

NP-complete Problems on Chordal Graphs
- Lemma. Given a perfect elimination ordering, the coloring problem and maximum clique problem can be solved in polynomial time.
	- pf>
		- V를 G의 PEO라고하자.
		- Color set $\mathcal{C}=\{1,2,\dots,k'\}$에 대해서 
			- v의 이웃 중 v이전에 오는게 k'개 미만이면 이웃들 중 없는 색을 써서 색칠
			- v의 이웃 중 v이전에 오는게 k'개면 새로운 색(k'+1)을 색칠하고 set에 추가한다
		- coloring problem을 풀면 color set크기가 maximum clique의 크기가 됨
- Lemma. Given a perfect elimination ordering, the independent set problem can be solved in polynomial time
	- PEO에 맞춰서 앞에서부터 greedy하게 선택하면 됨
	- pf>
		- OPT와 Greedy가 다르다고 가정하자
		- Greedy를 PEO순으로 놨을 때 $v_{i}$부터 다르다고 하고 이때 Greedy와 달랐던 OPT의 vertex를 $u_{i}$라고 하자.
		- 그러면...
			- $u_{i}$가 $v_{i}$보다 앞: greedy 선택의 방법에 의해 $u_{i}$와 이웃인 vertex가 $u_{i}$앞에 존재함 -> 모순
			- $u_{i}$가 $v_{i}$보다 뒤: OPT$\cup\{v_i\}$가 independent set이 아니므로(OPT가 최대크기) $v_{i}$와 연결된 OPT의 vertex가 있다. 특히나 PEO의 정의에 의해 $v_{i}$의 이웃이면서 뒤에 있으면 clique를 이루므로 딱 하나만 OPT에 포함될 것이다. 이 vertex를 $u_{i+k}$라고 하고 $u_{i+k}$를 $v_{i}$로 치환한 걸 OPT'라고하자. 그러면 |OPT'|=|OPT|인데 공통부분(prefix)가 늘어났다. 이를 반복하면 Greedy와 OPT가 같은 길이임을 보일 수 있다. 
