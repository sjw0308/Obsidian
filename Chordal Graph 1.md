
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
		- Claim 1을 이용해서 G에서 simplicial vertex를 v라고 할 때 G-v가 chordal임을 보이면 induction으로 증명을 할 수 있다. (Proof 2)
사전작업
- 정의: 모든 non-adjacent vertex x,y에 대해서 어떤 set S가 G-S에서 x,y가 disconnect로 만든다면 S를 x-y separator라고 한다
- Lemma. A chordal graph has two non-adjacent simplicial vertices, or it is clique
	- 이 Lemma가 Claim 1보다 더 강함
	- pf
		- Claim 2: For any two non-adjacent vertices x, y of G, its minimal x-y separator is a clique
		- Claim 2가 성립한다고 가정하면
		- 12페이지부터 계속 해야합니당~