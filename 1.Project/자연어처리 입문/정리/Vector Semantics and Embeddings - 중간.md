
기계에게 단어는 따로 뜻이 없다 
lexical semantics (단어 어휘학)
- 단어의 의미와 단어 사이 관계를 연구
- Lemma and senses
	- lemma(단어)에 대한 senses(뜻들)
	- lemma는 여러 의미를 지닌 polysemous(다의어)일 수 있다. 
- Synonymy (동의어/유의어)
	- big/large는 유의어지만 my big sister != my large sister
- Similarity
	- 비슷한 뜻을 지닌 단어
	- car/bicycle, cow/horse
- Word relatedness
	- coffee/tea : similar
	- coffee/cup : related, not similar
	- Semantic field
		- 하나의 단어에서 연관된 단어들의 field
- Antonymy(반의어)
	- slow/fast 처럼 binary opposition이거나 반대 scale을 향함
	- rise/fall처럼 reversive일 수도
- Connotation
	- 단어가 가진 뉘양스가 의미에 영향을 줄 수 있음
	- copy/fake : 긍정적/부정적
	- 또는 감정에서 반대될 수도 있다 love/hate
	- 이는 3가지 차원으로 분류한다
		- valence: 높음 love ~ toxic 낮음
		- Arousal(각성도): elated(의기양양한) ~ mellow(부드러운)
		- Dominance: powerful ~ weak

Word embedding
- cat을 어떻게 numerically 나타낼까?
- one-hot vector?
	- vocab size와 같은 길이 -> 너무 큼
	- 1은 그냥 indexing으로 아무 의미가 없음
- 정답은 vector semantic!
Vector Semantic
- Idea
	- Idea 1: Defining meaning by linguistic distribution
	- Idea 2: Meaning as a point in space (아까 나눈 3가지 차원)
- 각 word를 vector로 표현한다
- word별로 보면 완전히 같은 단어가 다시 들어와야 하지만
	- embedding으로 보면 비슷하지만 본적 없는 단어를 handling할 수 있다!
- 2가지 embedding
	- tf-idf
		- sparse vectors
	- Word2vec
		- dense vectors
		- contextual embedding

Word and vectors
- 여러 개의 document가 input으로 들어오면 각 단어가 각 document별로 얼마나 사용되었는지 count
- 이후 document를 단어들의 vector로 나타내면 어떤 document끼리 비슷한지 알 수 있음
- 반대로 생각하면 단어의 vector를 document들로 나타낼 수 있음
- 더 쉽게는 단어의 앞뒤 k칸에 어떤 단어들이 있는지 count하여 이를 벡터로 사용가능
- 그럼 similarity는 내적으로 계산할 수 있음 (둘의 절댓값의 곱으로 나눠서 -1~1로 값을 얻음)
- 빈번히 등장하는데 쓸모없는 단어들(e.g. stop word)에 영향을 받게 된다!
	- word를 weighting하는 두가지 solution이 있다.
	- tf-idf: $w_{t,d}=tf_{t,d}\times idf_{t}$
	- PMI: $PMI(w_{1}, w_{2})=\log \frac{p(w_{1}, w_{2})}{p(w_{1})p(w_{2})}$
- tf-idf
	- $tf_{t,d}= \text{count}(t,d)$ 해당 단어 t가 document d에 얼마나 있었는지
		- 그냥 count 보다도 이렇게 많이 씀
		- $$
		tf_{t,d} = \begin{cases} 1+\log_{10}\text{count}(t,d) & \text{if count}(t,d)>0 \\ 0 & \text{otherwise}\end{cases} 
		$$
	- df: document count
		- t가 나온 document의 수
	- idf: Inverse document count
		- $idf_{t}=\log_{10}\left( \frac{N}{df_{f}} \right)$
		- 단어 t가 희귀할 수록 점수가 높아짐
	- tf-idf vector는 길고, sparse하다!
		- 대체안으로 짧고 dense한게 없을까?
- Word2vec
	- Approach: Predict if candidate word c is a neighbor
		- 아몰랑 중간엔 안나올듯!!!