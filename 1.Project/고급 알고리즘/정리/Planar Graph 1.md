
Planar Graphs
- Def. 평면에서 모든 edges가 endpoint에서만 intersect를 만드는 graph이다. (평면에서 모든 edges가 서로 교차되지 않는 graph)
- Euler's Formula
	- G가 connected plane graph라면 $v-e+f = 2$ 이다. (v,e,f는 각각 vertices, edges, faces의 개수이다. Face는 edge로 나뉜 공간)
	- Pf>
		- case 1: G가 Tree일 때: cycle이 없어서 f=1, tree의 특성으로 v = e+1. $\square$
		- case 2
			1. G에서 어떤 Complete cycle을 생각하자
			2. 그 Complete cycle에서 edge를 하나 제거하자
			3. 그러면 Cycle은 없어졌지만 $v-e+f$값이 유지된다. (e가 1감소, f가 1감소)
			4. 이를 반복하면 결국 G와 같은 $v-e+f$값을 갖는 tree를 만들 수 있으므로 $\square$
	- Corollary
		- 어떤 planar graph G의 모든 planar embedding은 같은 수의 faces를 갖는다.
	- Corollary
		- For a connected, simple planar graph G with at least three vertices, we have $e\leq 3v-6$
		- simple: loop(edge하나로 자기 자신으로 돌아오는 edge)가 없고, 두 vertices사이를 잇는 edge가 최대 하나인 graph를 의미한다. 
		- Pf>
			- The sum of all face-boundary lengths is 2e.
			- Thus we have $3f \leq 2e$
			- 여기에 오일러 방정식을 적용하면 됨 $\square$
	- Corollary
		- $K_5$ 는 plannar가 아니다. (vertices가 5개인 완전연결 그래프)
	- Corollary
		- For a connected, simple planar graph G with at least $g$ vertices, where $g\geq 3$. If G has no cycle of length < g, $e\leq \frac{g}{{g-2}}(v-2)$
		- 이전 Corollary에서 3을 g로 바꾼 걸로 증명 가능
	- Corollary
		- $K_{3,3}$은 planar가 아니다. (vertices 3개인 set이 다른 set의 vertices와 모두 edge로 연결된 graph)
- Forbidden Graph 
	- A subdivision of a graph G
		- G에서 edge에 vertex를 껴 넣은 그래프를 G의 subdivision이라고 한다. 
	- Lemma. Planar graph는 $K_{5}$나 $K_{3,3}$의 subdivision을 subgraph로 갖지 않는다.
		- pf>
			- Planar graph의 subgraph 또한 plannar이다. 
			- G의 subdivision이 planar라면, G또한 planar이다. 
	- Theorem(Kuratowski). 그래프가 plannar이다. $\Leftrightarrow$ 그래프가 $K_{5}$나 $K_{3,3}$의 subdivision을 subgraph로 갖지 않는다.
- Forbidden minor
	- A minor of a graph G
		- G에서...
			1. subgraph이거나
			2. edge를 없애고, 연결하던 두 vertices를 하나로 merge
		- 위 두 작업을 반복해서 얻을 수 있는 그래프를 의미한다. 
	- Lemma. Planar graph는 $K_{5}$나 $K_{3,3}$의 subdivision을 minor로 갖지 않는다.
		- pf>
			- Planar graph의 subgraph 또한 plannar이다. 
			- G의 subdivision이 planar라면, G또한 planar이다. 
	- Theorem(Wagner). 그래프가 plannar이다. $\Leftrightarrow$ 그래프가 $K_{5}$나 $K_{3,3}$의 subdivision을 minor로 갖지 않는다.
- Fary's Theorem
	- 모든 simple planar graph는 모든 edge가 선분인 그래프로 planar embedding할 수 있다. (edge crossing 없이)
	- Pf> Using induction
		- Base Condition: vertex개수 3개 -> 자명
		- Induction Hypothesis: vertex개수가 n-1개인 maximal planar graph G가 선분으로 embedding할 수 있다고 하자.
			- maximal planar graph는 planar인데 하나의 edge만 추가해도 planar가 아니게 되는 graph를 의미한다. 
		- Induction Step
			- G가 n-vertex maximal planar graph라고 한다면, G의 모든 face는 triangle이다. (outer face 포함)
			- 따라서 $3f=2e$ 오일러 공식에 의해서 $e=3n-6$
			- $3n-6 = \sum_{v\in V}d(v)/2$ ($d(v)$는 degree로 연결된 edge수)
			- 이때 $d(v) \geq 3$ 이므로 degree가 5이하인 vertex가 적어도 4개 존재한다. (비둘기 집으로 쉽게 증명 가능)
			- 그런데 outer face가 triangle이므로 적어도 하나는 내부에 존재한다. 
			- 그 내부에 존재하는 vertex와 연결된 edge까지 없애면 IH에 만족하는 graph가 남는다. 
			- IH를 쓰고 마지막에 없앤 vertex와 edge를 추가하면 끝
			- (아마도 degree가 5이하 + 내부면 몇몇 캐이스로 특정이 가능해서 증명이 가능하기 때문인듯)
