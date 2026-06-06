- DFA는 unique language로 표현되지만 하나의 language를 표현하는 DFA는 여려가지 이다. 
- Given a regular language L, find the DFA with the fewest states accepting L

- Indistinguishable State
	- 두 state p, q가 다음을 만족하면 indistinguishable하다고 한다.
		- $\hat{\delta}(p,w)\in F$ implies $\hat{\delta}(q,w)\in F$
		- $\hat{\delta}(p,w)\notin F$ implies $\hat{\delta}(q,w)\notin F$
		- for all $w\in\Sigma^*$ 
- An Efficient State Minimization
	1. Remove all inaccessible states
	2. Make a table by considering all pairs of states (p, q)
	3. If $p\in F$ and $q\notin F$ or vice versa, mark the pair (p, q) as distinguishable
	4. Repeat the following step until no previously unmarked pairs are marked. For all pairs (p, q) and all $a\in\Sigma$ compute $\delta(p, a) = p_a$ and $\delta(q, a) = q_a$. If the pair ($p_a, q_a$) is marked as distinguishable, mark (p, q) as distinguishable
	- step 2 to 4 called Table-filling algorithm
	5. Combine indistinguishable states and make a reduced DFA
- Proof of There is no Unrelated Smaller DFA
	- Let A be our minimized DFA; let B be a smaller equivalent
	- Indistinguishable한 두 state에서 각각 transition한 두state는 서로 indistinguishable함을 이용하여 induction
	- Basis: start states of A and B are indistinguishable, becuase L(A) = L(B)
	- Induction
		- Suppose w = xa is shortest string getting A to state q.
		- By the IH, x gets A to some state r that is indistinguishable from some state p of B
		- Then, $\delta_A(r,a) = q$ is indistinguishable from $\delta_B(p,a)$
		- Thus all states of A and B are indistinguishable. So the number of states are same.
- 