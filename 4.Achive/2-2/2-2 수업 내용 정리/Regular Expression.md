DFA/NFA가 Regular language를 인식하는 machine-like description이라면 Regular expression은 더 user-friendly한 Regular language의 표현법이다.

- RE의 구성요소
	- String of symbols from $\Sigma$
	- 소괄호
	- Operators +, $\cdot$, \*
- Inductive Definition of RE
	- $\phi, \epsilon\ \ and \ \ a\in\Sigma$ are all regular expressions. 이들을 primitive RE라고 한다. 
	- If $r_1\ \ and \ \ r_2$ are RE, so are $r_1+r_2,\ \ r_1\cdot r_2,\ \ r_1^*,\ \ (r_1)$. 마지막 expression은 r_1이 expression 일 때를 염두해두고 표시한 것
	- A string is a RE if and only if to can be derived from primitive regular expressions by finite number of applications of the rules described above.
- Language of RE
	- The language L(r) denoted by any RE r, is defined by the following rules
	- $\phi$ is a RE denoting the empty set
	- $\epsilon$ is a RE denoting {$\epsilon$}
	- For every $a\in\Sigma$, a is a RE denoting {a}
	- $L(r_1 + r_2) = L(r_1) + L(r_2)$
	- $L(r_1r_2) = L(r_1)L(r_2)$
	- $L((r_1)) = L(r_1)$
	- $L(r_1^*) = (L(r))^*$

- Connection between Regular expressions and Regular Languages
	- For every regular language, there is a regular expression and vice versa
	- Proof 1(RE$\rightarrow$NFA): $\phi,\ \epsilon,\ \ a \in \Sigma$에 대하여 NFA표현 이후 이들 사이 연산을 NFA를 이용하여 만든 NFA로 설명한다. 
	- Generalized NFA: edge의 label이 RE이다. 이를 이용하여 기존 NFA들의 state를 줄여나갈 수 있는데 state를 최종적으로 줄여서 2개의 state가 남고 더 이상 expression을 줄일 수 없을 때를 Canonical Form of GNFA라고 한다. Canonical Form of GNFA는 RE로 변환 할 수 있다. 
	- Proof 2(NFA$\rightarrow$RE): NFA를 state를 줄여나가서 Canonical Form으로 만들고 RE로 변환

- Algebraic laws for RE
	- 교환법칙: Union에서는 성립하지만 Concatenation에서는 성립하지 않는다. 
	- 결합법칙: Union과 Concatenation에서 모두 성립한다. 
	- 분배법칙: 성립한다. 
	- Identity of Union: $\phi$, Identity of Concatenation: $\epsilon$
	- Annihilator(역원) of Concatenation: $\phi$
	- Idempotent(멱등성, 자기자신에 대하여 연산을 적용했을 때 결과가 자기자신이 나오니ㅡㄴ 것을 의미한다): Union에서는 성립하지만 Concatenation에서는 성립하지 않는다. 
	- Laws with Closures
		- $(r^*)^* = r^*,\ \phi^* = \epsilon,\ \epsilon^* = \epsilon,\ \ r^+ = rr^* = r^*r,\ \ r^* = r^+ + \epsilon$ 