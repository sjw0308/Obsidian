#### 오늘 보이고 싶은 내용: No algorithm for the 3-coloring problem on planar graphs runs in $2^{O(\sqrt{ n })}$ time unless ETH fails

기본개념 
- SAT problem
	- $(x\bar y z\bar u \bar z)(\bar x y z u \bar z)\dots$와 같이 논리식이 있을 때
	- 각변수 x, y, ...에 T/F 를 넣어서 논리식을 참으로 만드는 문제이다. 
	- 3-SAT 문제는 각 논리식에 최대 3개의 변수만 존재하는 SAT문제이다. 
- Hardness Assumption
	1. $P \neq NP$
		- 3-SAT 문제를 푸는 polytime 알고리즘이 없다
	2. Exponential-Time Hypothesis (ETH)
		- 3-SAT 문제를 푸는 subexponential-time 알고리즘이 없다
	3. Strong Exponential-Time Hypothesis (SETH)
		- 뭐시기뭐시기


1. No algorithm for the 3-coloring problem on general graphs runs in polynomial time unless P=NP
	- 3-SAT 문제를 3-coloring문제로 바꾸면 증명 끝임
	- 변수 $x_{i}$에 대해서 $x_{i}, \bar x_{{i}}$에 해당하는 vertex를 만들고 두 vertex와 B를 모두 연결한다. 추가로 T/F vertex를 만들어서 마찬가지로 서로와 B를 모두 연결한다. 
	- ![[Pasted image 20261017171245.png]]
	- 그리고 하나의 clause에 있는 t1~t3에 해당하는 vertex들을 아래의 모양으로 TF와 연결한다. 
	- ![[Pasted image 20261017171345.png]]
	- 그러면 t1~t3가 모두 false일 때 3-coloring이 불가능해진다. 
	- 이렇게 모든 clause를 연결하고 난 그래프를 구하고 이 그래프가 3-coloring이 polytime에 된다는 것은 3SAT문제를 polytime에 풀 수 있다는 것이므로 가정에 모순이 되어서 불가능하다

2. No algorithm for the 3-coloring problem on general graphs runs in $2^{O(n)}$ time unless ETH fail
	- Lemma. Let $\epsilon >0$ be a const. n variable dense일 수 있는 3SAT문제를 $2^{2\epsilon n}$ time 알고리즘을 통해서 $2^{\epsilon n}$개의 n variable, O(n) clauses (sparse)인 3SAT 문제로 바꿀 수 있다
	- 위에서 사용했던 치환을 이용할껀데, Lemma를 같이 이용해서 n개의 variable과 m개의 clause를 갖는 3SAT문제를 $2^{2\epsilon n}$ time 알고리즘을 통해서 $2^{\epsilon n}$개의 n variable, O(n) clause인 3SAT로 만들고 각각을 치환한다. 
	- 만약 n vertices, m edges인 graph의 3-coloring문제를 푸는데 걸리는 시간이 T(n,m)이라면 현재 3SAT문제를 푸는데 걸리는 시간은 $2^{3\epsilon n}*T(O(n),O(n))$ 이다. 
	- 따라서 $T(n,m)=2^{O(n)}m^{O(1)}$이면 3-SAT이 $2^{\epsilon n+O(n)}$에 풀려서 ETH에 위배된다

3. No algorithm for the 3-coloring problem on planar graphs runs in $2^{O(\sqrt{ n })}$ time unless ETH fails
	- 임의의 graph를 crossover gadget을 이용해서 edge의 교차지점을 대체함으로써 planar로 바꿀 수 있다. 
	- ![[Pasted image 20261017193221.png]]
	- intersection은 n에 대하여 $O(n^2)$개가 생길 수 있으므로 만든 planar graph는 vertex수가 $O(n^2)$가 될것이다. 
	- 여기에 2번을 적용하면 끝!