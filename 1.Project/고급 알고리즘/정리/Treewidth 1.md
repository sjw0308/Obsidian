
- Lemma. 모든 n-vertex tree는 T-c의 모든 component들의 크기가 n/2 이하가 되게 하는 vertex c가 존재하고 이를 centroid라고 한다. 
	- proof
	- 임의의 vertex v를 잡는다
	- component를 확인한다 
	- v가 centroid면 끝나고 아니라면 크기가 n/2보다 더 큰 component가 하나 있을텐데 해당 component에 있고, v와 연결된 vertex를 u라고 한다. 
	- u를 기준으로 하면 해당 component의 크기가 1줄어 들기 때문에 위를 반복하면 centroid를 찾을 수 있다
- Lemma. r x r grid는 minimum balanced separator $O(r)=O(\sqrt{ n })$ 이다
	- pf>
	- upper bound
		- 중간 row를 없애면 n/2보다 작은 크기의 component 2개로 나눠진다
	- lower bound
		- Separator S의 크기가 r/3보다 작다고 가정하면 적어도 r-s개의 row와 하나의 column이 S에 포함되지 않았을 것이다. 
		- 그러면 해당 조합으로 인해서 $r(r-s)>\frac{2r^2}{3} = \frac{2n}{3}$ 개의 vertex가 하나의 component가 된다.
- Planar graph가 아니더라도 작은 크기의 balanced separator를 가질 수 있다 (e.g. $K_{3,m}$의 balanced separator크기는 3)
- Lemma. Every tree admits a proper 2-coloring that we can compute in linear time

Tree decomposition
- Def
	- A tree decomposition of G = (V,E) consists of a tree T and a bag $B_{t}\subseteq V$ for each node t of T, satisfying the following conditions
		- Vertex coverage: G의 모든 vertex는 적어도 한 가방에 들어있어야 함
		- Edge coverage: 모든 edge $uv\in E$, 어떤 bag에는 u와 v가 모두 들어있어야 한다. 
		- Connectedness: 모든 vertex v에 대해서 v가 포함된 모든 bag은 연결되어 있어야한다. 
- Def
	- The width of a tree decomposition is the size of its largest bag minus one
	- the treewidth of G (tw(G)) 는 G의 모든 tree decomposition 중 width가 가장 작은 tree의 width를 의미한다
- Property
	- Every nonempty n-vertex graph satisfies $0\leq tw(G)\leq n-1$
		- nonempty bag + 하나의 bag에 모든 vertex를 넣은 경우
- Lemma
	- 모든 tree는 treewidth가 1이다.
		- 모든 vertex와 edge에 대한 bag을 만들고, 원래처럼 연결하면 width = 1

용어정리 Clique: 모든 vertex가 서로 연결된 그래프
Lemma (clique-bag lemma)
- 그래프 G의 모든 tree decomposition에 대해서 clique $Q\subseteq V(G)$는 하나의 bag에 들어가야 한다. 
- Pf>
	- $T_{v}$를 v가 포함된 모든 Bag으로 만들어진 tree decomposition의 sub-tree라고 하면
	- $\bar{uv}\in Q$에 대해서 $T_{u}\cap T_{v}\neq \emptyset$ . 이게 모든 $u,v\in Q$에 대해서 적용됨
	- Helly property에 의해서 증명 끝
Lemma (Helly property)
- If a finite collection of nonempty subtrees of a tree intersects pairwise, then all its members share a vertex (트리의 연결된 subtree들이 pairwise intersection을 가지면, 모든 subtree의 공통 교집합도 존재한다)
- Pf>
	- 임의의 $T_{i}$ 와 대해서 root의 depth가 가장 깊은 $T_{j}$가 있을 때 $T_{j}$의 root인 $h_{j}$가 intersection에 포함된다. 
		- 임의의 $x\in T_{i}\cap T_{j}$에 대해서 $h_{i}, h_{j}$는 모두 x의 공통 조상이다. 
		- $T_{i}$는 x를 포함하는 subtree임으로 $h_{j}$또한 포함하고 있어야 한다. 
	- 결과적으로 $h_{j}$는 모든 subtree에 포함된다
Lemma
- A graph G is a forest $\Leftrightarrow$ $tw(G)\leq 1$
- Pf>
	- $\Rightarrow$  앞에서 증명함
	- $\Leftarrow$  tw(G)가 1이하이면 G에 cycle이 없다는 것을 증명하자
		- 우선 모든 leaf bag $B_{t}$ 중 해당 bag과 연결된 다른 bag $B_{p}$에 대하여 $B_{t}\subseteq B_{p}$인 모든 $B_{t}$를 제거한다
			- 제거해도 $B_{t}$에 있는 정점과 edge는 $B_{p}$로 커버할 수 있음
			- tree decomposition 단순화
		- 그럼 모든 leaf bag $B_{t}$에 대하여 $B_{t}\nsubseteq B_{p}$이므로 leaf bag에는 이웃 bag에는 없는 vertex이 적어도 하나는 존재한다. 
		- 해당 vertex를 v라고 하면 해당 vertex는 $B_{t}$에만 존재한다.(다른데 존재하면 path가 생겨서 $B_{p}$)에도 존재하게 됨
		- 그럼 2가지 경우가 생기는데 $B_{t}$에 v만 있거나, v포함 vertex가 2개 존재하는 경우가 있음
		- 두 경우 모두 G에서 v를 없애도 tw(G)가 1이하임을 보장할 수 있다
		- 만약 G'=G-v가 forest라면? -> v를 붙이는데 $B_{t}$의 크기가 2이하 이므로 cycle이 생길 수 없어서 G또한 forest가 됨(v와 연결되는 edge가 최대 1개임)
		- 이걸 induction을 이용해서 증명하면 끝(그냥 vertex하나 있으면 forest임 -> G'가정 -> G에서 증명)
Lemma
- 모든 크기 3이상의 cycle에 대하여 $tw(C_{n})=2$
- Pf>
	- lower bound: cycle은 forest가 아님
	- upper bound
		- cycle의 vertex들을 $v_{1},\dots, v_{{n}}$ 라고 하면 bag들을 $\{v_{1},v_{i},v_{i+1}\},\text{ for i=2, ..., n-1}$로 잡으면 모두 커버된다. 

Treewidth under minor operations
- Property
	- H가 G의 minor라면, $tw(H)\leq tw(G)$
	- Pf1: deletions>
		- G의 tree decomposition에서 edge제거는 무시, vertex제거는 모든 bag에서 해당 vertex제거 -> 아직 모든 vertex와 edge가 커버됨. 같은 vertex도 여전히 연결되어 있음, tw안커짐
	- Pf2: contraction>
		- edge uv를 없애고 새로운 vertex x를 넣으면, bag에서 u, v가 있던 모든 부분에 x를 넣으면 됨 -> 마찬가지
- Consequence
	- G의 minor에서 $K_{r}$(clique)이 있으면 $tw(G)\geq r-1$

Weighted centroids in trees
- Def
	- T가 finite tree with nonnegative vertex weights totaling W라면 A **weighted centroid**는 vertex t s.t. every component of T-t has weight at most $\frac{W}{2}$이다
- Lemma
	- 모든 weighted tree는 weighted centroid가 있다. 
	- Pf>
		- W=0이면 모든 vertex가 성립
		- 임의의 시작 vertex t에 대하여 T-t의 component가 W/2보다 큰 weight sum을 가진다면 해당 component에 속한 neighbor쪽으로 이동(언제까지? 해당 component의 weight sum이 W/2보다 작아질 때까지 반대로 이동하지 않음  -> 유한한 횟수 안에 weighted centroid에서 종료)
Treewidth and balanced separators
- Lemma
	- If $tw(G)\leq k$, G의 모든 subgraph H가 size at most k+1 leaving components of size at most |V(H)|/2 인 separator를 갖는다. 
	- Pf>
		- 최대 width k인 bag으로 H를 decompose한 $(T, \{B_{t}\})$가 있다. 
		- H의 vertex들을 해당 vertex가 소속된 bag들 중 하나에만 assign하고, 각 bag을 배정된 vertex의 수로 weighted한다. 그럼 total weight = n = |V(H)|이다.
		- 거기서 weighted centroid를 t를 찾고 $S=B_{t}$라고 하자
		- 임의의 vertex $v\notin S$에 대하여 decomposed tree에서 v를 포함하는 subtree이면서 t를 포함하지 않는 (T-t의 한 component)를 생각하자
		- H-S에서 하나의 component를 이루고 있다면 decomposed tree에서도 같은 component를 이루고 있어야 한다. 
		- 그런데 T-t가 weighted centroid의 정의에 의해서 component의 weight sum의 크기가 n/2이하이므로 H-S의 component의 크기 또한 n/2이하가 된다. 
- Theorem (Dvořák-Norin)
	- G의 모든 subgraph H가 separator of size at most the same bound s, leaving components of size at most $\frac{2|V(H)|}{3}$를 갖는다면, $tw(G)=O(s)$이다. (증명 안함)
	- 이 thm이 말하고 싶은 것: G가 작은 balanced separator를 가지고 있음 $\neq$ tw(G)도 작음. 
		- G뿐만이 아니라 G의 모든 subgraph가 가져야 한다
- Theorem (Korhonen)
	- 주어진 vertex수가 n인 graph G와 integer k에 대하여 우리는 $2^{O(k)}n$시간 알고리즘을 통해서 width가 최대 2k+1인 tree decomposition 또는 $tw(G)>k$ 여부를 알 수 있다. (증명 안함)
	- 그러니까 해당 알고리즘이 tree decomposion을 반환한 경우는 tw(G)가 2k+1이하라는 걸 알 수 있고, 해당 알고리즘이 tree decomposion에 실패하면 tw(G)가 k초과이다 라는 사실을 알 수 있음
	- 정확한 treewidth를 알아내는건 NP-hard이지만 고정된 k에 대해서 위의 알고리즘을 돌리는 것은 O(n)이다

3 coloring
- Def
	- Root a width-k tree decomposition (T, {$B_{t}$}) at r. Let $V_{t}$ be the union of all bags in the subtree rooted at t, and let $G_{t}=G[V_{t}]$. For each assignment $\varphi:B_{t}\rightarrow{1,2,3}$, define $D_{t}(\varphi)$ to be true exactly when $\varphi$ extends to a proper 3-coloring of $G_{t}$
	- 쉽게 정리하면, (T, {$B_{t}$})에서 임의의 bag을 root r로 잡음
	- 그럼 임의의 bag t에 대한 subtree가 정의 되는데 여기에 속한 모든 bag의 vertex들의 합집합을 $V_{t}$라고 하고 이걸로 이루어진 subgraph를 $G_{t}$라고 할꺼임
	- 이게 t가 담당하는 subgraph가 되는거임
	- $\varphi$는 bag에 들어있는 vertex들을 1~3으로 색칠하는 함수인데 이때 $D_{t}(\varphi)$를 $\varphi$를 유지하면서 $G_{t}$전체를 올바르게 3-coloring할 수 있으면 true, 아니면 false를 반환하는 함수로 만들었음. 까지가 정의
- Interpretation
	- $B_{t}$는 $V_{t}$에 속한 vertex들이 outside와 연결되는 boundary역할을 함
	- 그러니까 DP를 이용할 때 해당 boundary의 색 배정만 신경쓰면 됨
- Algorithm
	- Compute the tables from the leaves toward the root. Each table has at most $3^{k+1}$ entries. Since $V_{r}=V(G)$, accept exactly when $D_{r}(\varphi)$ is true for some assignment $\varphi$
	- leave bag부터 root bag으로 계산할꺼임
	- 이때 각 bag의 $\varphi$의 가능성은 tree decompositionn의 width가 k이므로 $3^{k+1}$이하가 됨
	- 결과적으로 root node r에 대한 $D_{r}(\varphi)$값이 이 그래프를 3-coloring할 수 있냐 없냐 가 됨
- Property for Recurrence
	- t의 child u에 대해서 $V_{u}\cap B_{t}=B_{u}\cap B_{t}$이다. 
	- 아까 말했던 boundary의 의미
	- 내부($V_{u}\setminus B_{u}$)의 정점과 $B_{t}$에 같은 노드가 있는데 $B_{u}$에 없으면 tree decomposition 정의에 모순됨
- Recurrence
	- Set $D_{t}(\varphi)=true$ if and only if $\varphi$ properly colors $G[B_{t}]$ and, for every child u, there is an assignment $\psi$ with $D_{u}(\psi)=true$ that agrees with $\varphi \ on\ B_{t}\cap B_{u}$
	- $D_{t}(\varphi)=true$ 라는 건 현재 bag도 적절히 색칠했고
	- 모든 child u에 대해서 child 공통 vertex와 색이 같으면서 $D_{u}(\psi)=true$인 $\psi$가 존재한다는 의미이다. 
- Base case
	- At a leaf t, $V_{t}=B_{t}$. Thus $D_{t}(\varphi)$ is true exactly when $\varphi$ properly colors the edges inside $B_{t}$
- Lemma for Correctness
	- For every node t and assignment $\varphi$ on $B_{t}$, the recurrence returns true if and only if $\varphi$ extends to a proper 3-coloring of $G_{t}$
	- Pf>
		- root가 t인 subtree에서 induct를 해보자
		- 우선 leaf는 $G_{t}=G[B_{t}]$이므로 base case는 correct
		- 실제 색칠 가능이면 dp가 true임을 증명
			- 3-coloring 가능이면 $f:V_{t}\rightarrow\{1,2,3\}$이 존재한다는 의미임. 또한 $f|_{B_{t}}=\varphi$
			- 이때 t의 임의의 child u에 대해서 $f|_{V_{u}}$가 존재하고 $\psi=f|_{B_{u}}$라고 하자
			- 그러면 $D_{u}(\psi)=true$이므로 귀납가정이 성립한다. 
			- 마지막으로 $\forall v \in B_{t}\cap B_{u}$에 대하여 $\varphi(v)=\psi(v)$이다. 
		- dp가 true이면 실제 색칠 가능임을 보이자
			- $D_{t}(\varphi)=True$이므로 아래 두개가 성립함
				- $\varphi$는 $B_{t}$를 올바르게 색칠한다
				- 모든 child u에 대해 $\varphi$와 agree이면서 $D_{u}(\psi_{u})=True$인 $\psi_{u}$가 존재한다 
			- 이전 Property for Recurrence에 의해서 child끼리만 agree하면 서로의 색칠을 합칠 수 있다. 
			- 또한 $G_{t}$의 모든 edge는 $G[B_{t}]$안에 있거나 $G_{u}$에 있으므로 합친 색칠은 proper coloring이다
- Thm
	- Given a tree decomposition of width k with N bags, we can decide 3-colorability in $2^{O(k)}(n+N)^{O(1)}$
	- Pf>
		- 모든 bag은 최대 k+1 vertex이므로 최대 $3^{k+1}$ assignment를 갖는다.
		- parent-child pair에서 agreement를 볼때 최대 $3^{2(k+1)}$의 쌍을 비교한다. 
		- bag내부 edge에 대한 확인은 polytime이 걸림
- Corollary
	- If $tw(G)\leq k$, the decomposition algorithm gives width at most 2k+1 in $2^{O(k)}n$ time. Substituting this width into DP then $2^{O(k)}n^{O(1)}$ algorithm for 3-coloring