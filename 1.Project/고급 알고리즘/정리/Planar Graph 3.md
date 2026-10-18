Lemma. A planar graph has balanced separator of size $O(\sqrt n)$
- Def. G = (V,E)의 Balanced separator를 S라고 하면 G\[V-S]의 모든 component의 크기가 $\frac{2n}{3}$이하로 만드는 Set을 의미한다. 
- Pf
	1. Triangulation
		- maximal planar graph G=(V(크기 n), E)에서 balanced separator가 있다면 임의의 planar graph에서도 존재한다. 
		- pf: 임의의 G에 edge를 추가해서 만든 maximal planar graph에 balanced separator가 있다면 해당 balanced separator는 G에도 적용된다. $\square$
	2. Find a small balanced separator for a planar graph with small diameter
		- 임의의 두 vertex 사의의 거리가 $O(\sqrt n)$인 graph에서 balanced separator가 존재함을 증명하자
		- pf
		- 임의의 vertex v에 대한 BFS Tree를 T라고 하자. 그러면 T의 layer는 $O(\sqrt n)$로 bounded된다.
		- fundamental cycle C(e)를 non-tree edge e=ab에 대하여 각각 a,b에서 가장 가까운 공통조상까지 path와 e로 이루어진 cycle이라고 하면 C(e)의 길이는 $O(\sqrt n)$로 bounded된다. 
		- C(e) 내부의 vertex들을 int(e), 외부를 ext(e)라고 하자 (둘다 C(e) 포함)
		- 우리가 하고 싶은건 둘다 크기가 $\frac{2}{3}n$보다 작아졌을 때 C(e)를 balanced separator로 취하고 싶은 것!!! 그러므로 둘 중 하나가 $\frac{2}{3}n$보다 클 때 그 값을 줄여가서 $\frac{2}{3}n$보다 작게 만들고 싶다!!
		- 이때  G가 maximal이기 때문에 e를 포함하는 두개의 triangle face가 있을 것이다. 각 face는 int(e), ext(e) 중 하나쪽에 있을 텐데 그 중 더 큰쪽에 있는 Face를 이루는 edge들을 $e_{1}, e_{2}$라고 하자
		- 그럼 e1, e2가 모두 tree edge일 수는 없기 때문에 둘다 non-tree edge이거나 e1만 tree edge인 경우가 존재한다. 
		- 먼저 e1만 tree edge인 case는 e를 e2로 대체해서 $max\left\{ |int(\cdot)|,|ext(\cdot)| \right\}$값을 1만큼 줄일 수 있다. 
		- 둘 다 non-tree edge인 case에서 e1, e2 중 $max\left\{ |int(\cdot)|,|ext(\cdot)| \right\}$ 값이 더 큰 edge를 e1이라고 하면 $max\left\{ |int(e_{1})|,|ext(e_{1})| \right\} < max\left\{ |int(e)|,|ext(e)| \right\}$임을 보일 수 있다. 따라서 e를 e1으로 대체해서 $max\left\{ |int(\cdot)|,|ext(\cdot)| \right\}$ 값을 1이상 줄일 수 있다.
		- 이렇게 대체하는 것을 반복해가면 결국 우리가 원하는 결과( $max\left\{ |int(\cdot)|,|ext(\cdot)| \right\} < \frac{2}{3}n$을 만족하게 하는 e 찾기)를 얻을 수 있다. 
	3. Reduce the original problem to the problem in step2
		- 임의의 vertex v에 대한 BFS Tree T를 생각하자. 그럼 그 tree의 layer는 n'개 있을 것이다. 
		- 레이어들 중에서 자신의 위쪽 vertex의 수와 아래쪽 vertex의 수가 최소가 되게 하는 layer를 median layer라고 하자 ($L(i_{m})$) (위 아래의 vertex의 수가 모두 $\frac{2}{3}n$보다 작다)
		- 우리는 이 median layer를 balanced separator로 쓰고 싶은데 해당 레이어의 vertex의 숫자가 $O(\sqrt{ n })$에 bounded가 아닌 것이 문제이다. 
		- 그럼 median layer에서 가까운 layer 중 layer의 vertex의 수가 $O(\sqrt{ n })$으로 bounded된 layer 중 root쪽에 가까운 것을 $L(i_{r})$ leef쪽에 가까운 것을 $L(i_{l})$이라고하자
		- 이때 그럼 $i_{r},~ i_{m}$사이 레이어들은 모두 $O(\sqrt{ n })$에 bounded가 되지 않은 것이므로 둘의 차이는 $O(\sqrt{ n })$이하이다. ($i_{l}$쪽도 마찬가지)
		- 그래서 우리는 일단은 separator S로 $L(i_{r})\cup L(i_{l})$을 잡자. 그럼 tree가 3부분으로 나뉘게 된다
		- 우선 0~ir부분은 0~im까지가 $\frac{2}{3}n$보다 작으므로 그것보다 작아서 작음. 마찬가지로 il~n'부분도 $\frac{2}{3}n$로 bounded이다. 
		- ir~il부분은 깊이가 $O(\sqrt{ n })$로 bounded이기 때문에 가상의 root와 step2를 이용하면 그 부분의 separator인 S'을 얻을 수 있다. 
		- 그럼 최종적으로 $S\cup S'$이 Balanced Separator가 된다. $\square$

Theorem. 3-coloring can be solved in $2^{O(\sqrt n)}$ time for planar graphs.
- 색칠 문제를 위 Lemma를 이용해서 S로 나누고, S로 나뉜 companent들($T(n_{i})$)을 생각하면 아래와 같이 생각할 수 있다. 
- S를 그냥 $3^{O(\sqrt{ n })} = 2^{O(\sqrt{ n })}$으로 노가다 하고 나머지 component들로 나누면
- $T(n) \leq 2^{O(\sqrt n)}(T(n_{1})+\dots+T(n_{k})),\ where\  n_{1}+\dots+n_{k} \leq n\ \&\ each\ n_{i}\leq \frac{2n}{3}$  
- 위 과정을 recursive하게 반복하면 결국 $2^{O(\sqrt{ n })}$ 로 bounded 됨 $\square$ 
