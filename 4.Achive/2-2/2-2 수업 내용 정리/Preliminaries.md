- Formal Proof
	- 모두가 완벽히 이해할 수 있도록 precise artificial language로 쓰인 증명
	- Mathematical proof는 formal proof보다 더 축약적이고 직관이 필요한 증명이다. 
	- proof방법에는 특정 steps에 끝나는 deductive방법과 inductive방법이 있다. 

- Set
	- A collection of elements without any structure other than membership
	- Subset: $A\subseteq B$ , Proper subset: $A\subset B$, Union: $A\cup B$, intersection $A\cap B$, Complementation: $\bar{A}$, Difference: $A-B$, Disjoint: $A\cap B = \phi$ 
	- Power set of S($2^S$): The set of all subsets of a set S
	- Cartesian product: $S = S_1 \times S_2 = \{ (x, y) | x\in S_1, y\in S_2\}$ 

- Function 
	- Function f from set A to set B is an assignment of exactly one element of B to each element of A
	- A is domain of f, B is codomain of f
	- f(A) is range of f which is the set of all images of elements of A 
	- one-to-one(injective) function: $f(x) = f(y) \ \  implies \ \ x=y, \ for\ \forall x,y \in A$ 
	- onto(surjective) function: The range = B
	- one-to-one correspondence (bijective) function: injective + surjective

- Graph
	- Consist of two finite sets, V(a set of vertices) and E(a set of edges)
	- Walk: A sequence of edges. $(v_0, v_j)(v_j, v_k) , \dots ,(v_m, v_n)$is walk from $v_0\ \ to \ \ v_n$ 
	- Trail: A walk in which all edges are distinct. 겹치는 edge가 없는 walk
	- Path: A walk in which all the edges are distinct and the vertices are distinct(except $v_0 = v_n$)
	- Closed: $v_0 = v_n$
	- Cycle: A closed path containing at least one edge
- Tree
	- connected acyclic graph

- Proof techniques
	- Proof by deduction
		- consist of a sequence of statements whose truth leads us from the hypothesis to a conclusion statement
	- Proof by induction
		- Basis: Prove that P(1) is true (or P(1)~P(i) is true)
		- Induction step: For each $i\geq 1$, assume that P(i) is true and use this assumption to show that P(i+1) is true
	- Proof by Contradiction
		- Assume that the theorem is false. And show that this assumption leads to an obvious false consequence.