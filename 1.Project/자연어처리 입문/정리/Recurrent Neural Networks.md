
형태
- hidden layer를 recurrent하게 다시 hidden layer에 입력을 넣는 형태
종류 및 예시
- one to many
	- captioning
- many to one
	- classification
- many to many (입력을 다 받고나서 입력이 계속 나오는 형태)
	- translation
- many to many (입력을 받기 시작하면서 출력이 나오는 형태)
	- Physical AI, VLA

vanilla RNN
- $$a_{t}=W_{x}x_{t}+W_{h}h_{t-1} \\ h_{t}=\tanh(a_{t}) \\ y_{t}=W_{o}h_{t}$$
- 예제
	- $x_{1}=1\ x_{2}=2\ x_{3}=1$
	- $h_{0}=0$
	- $W_{x}=1\ W_{h}=1\ W_{0}=2$
	- 결론
	- $h_{1} = W_{x}x_{1}+W_{h}h_{0} = 1,\ h_{2}=3,\ h_{3}=4$

Backpropagation of RNN
- 전부 지나서 back propagation을 하면 1보다 작은 값은 계속 0에 가까워지고, 1보다 크면 너무 커짐
- 그래서 너무 길지 않은 텀마다 분할하여 back propagation을 진행한다. 
- 또는 gradient clipping을 이용한다. 해당 방법을 사용하면 너무 커지는 gradient flow를 막을 수 있다. $\nabla{L}\leftarrow min\left( 1, \frac{c}{\|\nabla L\|} \right)\nabla L$

LSTM
- Gates (sigmoid)
	- Forget gate $F_{t}$
		- 이전 기억에서 얼마나 남길지를 선택
	- Input gate $I_{t}$
		- 이번 input에서 얼마나 기억할지 선택
	- Output gate $O_{t}$
		- 기억 중 무엇을 지금 꺼내서 사용할지를 선택
- Candidate memory $\tilde{C}_{t}$
	- $\hat{c}_{t}=\tanh(W_{xc}x_{t}+W_{hc}h_{t-1})$
- hidden state에서 넘어가는 정보
	- 원래 넘어가던 것
		- $h_{t}$ : short term memory에 해당하는 것
		- $h_{t}=o_{t}\odot \tanh(c_{t})$
	- Memory cell
		- $c_{t}$ : long term memory에 해당하는 것
		- $c_{t}=f_{t}\odot c_{t-1}+i_{t}\odot\hat{c}_{t}$

GRU
- Gates (sigmoid)
	- Reset gate $R_{t}$
	- Update gate $Z_{t}$
- Candidate hidden state $\tilde{H}_{t}$
	- $\hat{h}_{t}=\tanh(W_{xh}x_{t}+W_{hh}(r_{t}\odot h_{t-1}))$
- hidden state에서 넘어가는 정보
	- $h_{t}=z_{t}\odot h_{t-1}+(1-z_{t})\odot \hat{h}_{t}$

Attention
- ![[ChatGPT 이미지 2026년 11월 9일 오전 01_02_28.png|309]]
- $$c_{i}=\sum_{j=1}^{T_{x}}\alpha_{ij}h_{j} \\ \alpha_{ij}=\frac{\exp(e_{ij})}{\sum_{k=1}^{T_{x}}\exp(e_{ik})} \\ e_{ij}=a(s_{i-1},h_{j})$$
- 위에서부터 각각 +, softmax, NET을 의미한다. (예제 참고)
- 직전 state의 상태($s_{i-1}$)에서 각 입력에 대한 관련성 점수인 $e_{ij}$를 구함. 그리고 이 점수들을 softmax로 적용해서 attention weight($\alpha_{ij}$)를 구한다. 이러면 input의 어떤 부분이 중요한지의 점수가 weight로 적용되서 나온다. 이후 마막으로 이 점수들을 비중으로 input을 합침

Sampling
- Attention으로 다음에 나올(결과로 나올) token들의 확률(점수)가 나온다. 이중 어떤 결과를 선택는 방법이다. 
- Greedy
	- 가장 점수 높은 걸로 선택
- Beam search
	- Beam size 개 만큼의 높은 점수를 선택한다. 
	- 이때 중요한 점은 여러개의 토큰을 이어서 선택할 때는 선택한 토큰에서 이어지는 전체 결과 중 total점수가 가장 높은 걸 선택해야한다. 
- Standard sampling
	- 점수에 해당하는 확률을 주고 이에따라 랜덤으로 뽑음
	- 이때 점수를 $\tau$로 나누어서 해당 값이 작아지면 높은 확률에 더 가중치를 주는 것이고, $\tau$값이 커지면 모든 token을 더 고르게 고르는 것이다. 
- Top-K, Top-P
	- Top-K
		- 점수가 높은 k개의 token만 가지고 해당 점수들을 다시 정규화해서 선택하는 방법
		- 이 방법은 k개보다 작은 수의 token들이 특히나 점수가 클 때 점수가 작은 token들까지 함께 고려하게 되는 문제가 있다
	- Top-P
		- 점수 순으로 정렬했을 때 상위 P확률에 걸리는 token들의 점수를 다시 정규화해서 선택하는 방법