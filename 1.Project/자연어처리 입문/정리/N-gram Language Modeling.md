
Language Modeling more formally
- Goal: 문장이나 sequence의 확률을 구하기; $P(W)=P(w_{1},w_{2}, \dots , w_{n})$
- Related task: 이전에 온 것들로 다음 단어 추측하기; $P(w_{n}|w_{1}, w_{2}, \dots, w_{n-1})$
- LM은 위 두개 중에 하나를 계산해야 한다. 
- The Chain Rule: $P(w_{1}, w_{2}, \dots, w_{n}) = P(w_{1})P(w_{2}|w_{1})P(w_{3}|w_{1},w_{2})\dots P(w_{n}|w_{1}, \dots, w_{n-1}) = \Pi^{n}_{{k=1}}P(w_{k}|w_{1:k-1})$
Markov Assumption
- $P(w_{n}|w_{1:n-1}) \approx P(w_{n}|w_{n-1})$
- Bigram Markov Assumption
	- $P(w_{1:n}) = \Pi^{n}_{{k=1}}P(w_{k}|w_{k-1})$ instead of $\Pi^{n}_{{k=1}}P(w_{k}|w_{1:k-1})$
- More generally N-gram
	- $P(w_{n}|w_{1:n-1}) \approx P(w_{n}|w_{n-N+1:n-1})$
	- n을 포함해서 N개인 것을 기억하자
	- 한계점
		- long-distance dependencies를 확인하기 힘들다
		- 비슷한 뜻을 지닌 새로운 문장에 잘 대응할 수 없다

Estimating bigram probabilities
- The Maximum Likelihood Estimate: $P(w_{n}|w_{n-1})=\frac{C(w_{n-1}w_{n})}{\sum_{w}C(w_{n-1}w)} = \frac{C(w_{n-1}w_{n})}{C(w_{n-1})}$ 
- 문장이 이어지면 확률이 곱해지면서 너무 작아짐
	- log를 사용하여 scale을 확인

Evaluate N-gram models
- Extrinsic Evaluation
	- 두 모델을 비교하는 것
	- 한계가 큼
- Intrinsic evaluation: perplexity
	- dataset을 training set과 test set으로 나눠서 test set은 unseen dataset으로 나중에 모델을 evaluation metric을 이용해서 내부적으로 점검
	- training and test set은 task를 reflect하는 set이어야 함
	- 갑자기 정확도가 높아지면 training set의 오염(test set과 섞인 것)을 의심해야함
	- Dev sets
		- Model 훈련 중 사용하는 점검 set
		- Dev set은 여러번 사용해서 모델 훈련 중 점검에 활용하고, 가장 마지막에만 test set을 한 번 활용해서 모델을 평가한다. 
	- Perplexity: $PP(W) = P(w_{1}w_{2}\dots w_{N})^{-\frac{1}{N}}=\sqrt[N]{\frac{1}{P(w_{1}w_{2}\dots w_{N})}}$
		- lower perplexity = more probability = better!
		- Chain rule: $PP(W)=\sqrt[N]{\displaystyle\prod^N_{i=1}\frac{1}{P(w_{i}|w_{1}w_{2}\dots w_{i-1})}}$
		- Bigrams: $PP(W)=\sqrt[N]{\displaystyle\prod^N_{i=1}\frac{1}{P(w_{i}|w_{i-1})}}$
