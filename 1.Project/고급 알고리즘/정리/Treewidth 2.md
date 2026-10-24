
Algorithms on a tree decomposition
- Given a tree decomposition of width w, 아래 문제들을 DP를 이용해서 $2^{O(w)}n^{O(1)}$에 풀 수 있다. 
- Maximum Independent Set, Minimum Vertex Cover, Minimum Dominating Set, Max Cut, Odd Cycle Transversal, Feedback Vertex Set, Hamiltonian Path, Hamiltonian Cycle, Longest Path, Steiner Tree

The Grid Minor Theorem
- For every integer $r\geq 1$, there exists f(r) such that every graph G satisfies 
  $tw(G)\geq f(r)\Rightarrow Q_{r}\preceq G$ Here, $Q_{r}$ is an r x r grid, and $\preceq$ denotes the minor relationship
- treewidth가 충분히 큰 G는 r x r grid를 minor로 갖는다
- The best known bound: $\Omega(r^2\log r)\leq f(r)\leq O(r^9poly\log r)$
- For planar graph, $f(r)\leq 6r$ ($tw(G)\geq 6r\Rightarrow Q_{r}\preceq G$)
- We can compute a $Q_{r}$ minor in $2^{O(r)}n^{O(1)}$ time of a $Q_r$ minor exists

The win-win strategy for planar graphs
- Let $t=O(\sqrt{ k })$
	- 이전 내용: $tw(G)>t$를 보고하거나 $width\leq 2k+1$ 인 tree decomposition을 반환
	- if it reports $tw(G)>t$, the planar graph G contains $Q_{O(\sqrt{ k})}$ as a minor
	- Otherwise, we have a tree decomposition of width $O(\sqrt{ k })$
Example 1. Vertex Cover
- Compute a vertex cover of size k in a planar graph
- If it reports $tw(G)>t$
	- the planar graph G contains $Q_{\Omega(\sqrt{ k })}$ as a minor
	- then we can solve the problem immediately -> 불가능하다고 결론남
- Otherwise, we have a tree decomposition of width $O(\sqrt{ k })$
	- Then the DP algorithm runs in time $2^{O(\sqrt{  k})}n^{O(1)}$ time
Example 2. Feedback Vertex set
- Compute a feedback vertex set S of size k in a planar graph (i.e. S is a vertex set s.t. S is a forest)
- If it reports $tw(G)>t$
	- the planar graph G contains $Q_{\Omega(\sqrt{ k })}$ as a minor
	- Then we can solve the problem immediately
- ~~
Example 3. Longest Path (내용 동일)
Limitation of This Argument
- Dominating Set (graph G and integer k, compute a vertex set D s.t. for any vertex V of  G, either $v\in D$ or a neighbor of $v\in D$ size of $D\leq k$) has an efficient DP algorithm, but its optimum is not minor-monostone
- Dominating set 문제는 OPT(minor of G)가 OPT(G)보다 작지 않기 때문에 위의 방법론을 사용할 수 없다. 

Baker's Technique
- For $0<\epsilon<1$, given an n-vertex planar graph G, we can find an independent set I of size at least $(1-\epsilon )\cdot OPT$ (apprx soln) in $2^{O(1/\epsilon)}n^{O(1)}$ time
- Basic strategy
	- Set $q=\lceil \frac{1}{\epsilon}\rceil$. Delete one residue class of BFS layers, solve the remaining pieces exactly, and try all q choices
	- We will prove both ingredients: (i) the remaining pieces have treewidth O(q), and (ii) one choice loses at most 1/q(=$\epsilon$) of an optimum solution
- BFS Layers and Deleted Layers
	- G의 BFS tree의 layer를 $L_{1},\dots,L_{n'}$라고 하자
	- 각 $j \in \{1,\dots,q-1\}$에 대하여 $D_{j}=\bigcup_{i \equiv j(\mod q)}L_{i}$라고 할때, G에서 $D_{j}$를 없앤다
	- 그럼 최대 q-1개의 연속된 layer들이 이 존재한다. 
	- Claims
		- The treewidth of each block is at most 3q-1
		- $G-D_{j}$ 의 maximum independent set $I_j$들 중 최댓값이 $(1-\epsilon)\cdot OPT$ 이상이다
	- 만약 Claim들이 성립한다면 우리는 appox soln을 $2^{O(1/\epsilon)}n^{O(1)}$에 구할 수 있다
- Claim 1
	- If a planar graph H has a vertex s with $dist_{H}(s,v)\leq R$ for every v, then $tw(H)\leq 3R$
	- Pf>
		- H를 maximal planar라고 가정하자
		- BFS spanning tree T of H rooted at s 를 생각하자
		- tree내의 임의의 face를 f라고하면 maximal이므로 해당 face의 꼭짓점 a,b,c가 있을 텐데 s에서 해당 세 vertex까지의 path에 있는 vertex의 집합을 $B_{f}$ 라고 하자. 그럼 $|B_{f}|\leq 3R$
		- 각 face를 vertex로 하고, 두 face가 non-tree edge를 공유한다면 연결된 것으로 간주하면, 그렇게 만들어낸 그래프 D 또한 tree이고, 이를 co-tree라고 한다.
		- 그럼 D에 해당 bag을 배정하면 D가 tree decomposion이 되고 따라서 $tw(H)\leq 3R$이다
- Claim 2
	- The maximum of $I_{j}$ has size at least $(1-\epsilon)\cdot OPT$, where $I_{j}$ is the MIS of $G-D_{j}$
	- Pf>
		- layer를 제거하는 trial을 하는 과정에서 모든 vertex는 딱 한번씩 제외됨
		- 따라서 $I^*$를 OPT라고 하면, $\sum_{j=0}^{q-1}|I^* \cap D_{j}|=|I^*|$
		- 어떤 $j^*$에 대해서 $|I^*\cap D_{j^*}|\leq|I^*|/q$ 인 $j^*$가 존재함
		- set $I^* \setminus D_{j^*}$는 $G-D_{j^*}$의 MIS이다. 따라서 $|I_{j^*}|\geq|I^* \setminus D_{j^*}|\geq(1-1/q)|I^*|$
