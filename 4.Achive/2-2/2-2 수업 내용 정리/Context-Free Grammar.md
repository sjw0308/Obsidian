- Language의 포함 관계: Language > Turing Recognizable > Decidable > Context-free > Regular

- Context-Free Grammar
	- Context-free grammar의 의미는 이전 varieable에만 영향을 받고, 나머지(context)에 대한 영향을 받지 않는다는 의미이다. 
	- G=(V, T, S, P)에서 모든 P가 다음의 형태일 때 CFG라고 한다.  $P:\ \ A\rightarrow x\ \ where \ \ A\in V\ \ and \ \ x\in(V\cup T)^*$ 
- Derivation
	- productions를 이용하여 해당 string을 만드는 과정을 의미한다. 
	- 특정 string이 해당 CFG에 포함되는지 알기 위해서 진행한다. 
	- Types: Leftmost derivation, Rightmost derivation
		- Leftmost derivation은 가장 왼쪽에 있는 variable부터 production을 진행하는 것이고, Rightmost derivation은 가장 오른쪽부터 진행하는 것이다. 
	- Iterative Derivation (= Multi-step Derivation)
		- S $\Rightarrow \alpha$: One step derivation
		- S $\Rightarrow^{\star} \alpha$: Iterative derivation
	- Sentential Form
		- Any string of variables and/or terminals derived from the start symbol
		- S $\Rightarrow ^* \alpha$ 인 $\alpha$ (consist of any mix of terminals and non-terminal)

- Context-Free Language
	- The language of the grammar is said to be context-tree iff there is a context-free grammar G such that L = L(G), where $L(G) = \{w\in T^\star | S \Rightarrow^\star w\}$ 
	- A language is said to be context-free iff there is a context-free grammar G such that L = L(G), where $L(G) = \{ w \in T^* | S \Rightarrow ^* w\}$ 

- Linear Grammar
	- Grammar in which at most one variable can occur on the right side of any production without restriction on the position of this variable
- Non-linear Grammar
	- A derivation may involve sentential forms with more than one variable. In such cases, we have a choice in the order in which variables are replaced
	- Left-most Derivations
		- Say $wA\alpha \Rightarrow^*_{lm} w\beta\alpha$ is w is a string of terminals only and $A\rightarrow\beta$ is a production
	- Right-most Derivations
		- Say $\alpha Aw\Rightarrow^*_{rm} \alpha\beta w$ is w is a string of terminals only and $A\rightarrow\beta$ is a production