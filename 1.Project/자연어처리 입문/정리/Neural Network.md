
$y = \sigma(w_{1}x_{1}+\dots+w_{n}x_{n})$
우리는 w를 학습시켜야 한다. 

Loss
- 우리의 예측인 y와 정답인 $y^*$의 차이를 나타내는 함수
- 우리는 이를 줄이는 방향으로 학습시켜야 한다
- for regression: Squared Error
	- $\mathcal{L} = (y-y^*)^2$
- for classification: Negative Log-Likelihood
	- $\mathcal{L}=-\log\hat{y}$

Gradient Descent
- Loss를 이용한 학습
	- $w^{new}_{i} = w_{i}-\eta \frac{\partial\mathcal{L}}{\partial w_{i}}$
	- 여기서 $\eta$는 learning rate
- 이때
	- $\frac{\partial y}{\partial w_{0}}=\frac{\partial(w_{0}x_{0}+\dots+w_{d}x_{d})}{\partial w_{0}}=x_{0}$
- 여기서 bias를 추가하면 ($y=W\cdot X + b$)
- 만약 layer가 늘어나면? -> chain rule을 이용
	- $x \to y_{1}, y_{2} \to z$ 라고하면
	- $\frac{dz}{dx}=\frac{dz}{dy_{1}} \frac{dy_{1}}{dx}+\frac{dz}{dy_{2}}  \frac{dy_{2}}{dx}$
	- 위 식들을 이용한다. 
- Backpropagation
	- 결과에서 변화가 역전파되면서 결국 원하는 w,b의 gradient를 구하는 방법

About Learning part
- 언제 weight를 update할꺼냐?
	- Gradient Descent (Batch GD)
		- batch 전체를 한번에 학습하는 방법
		- 계산이 빡세고 느림
	- Stochastic Gradient Descent (SGD)
		- 모든 training point마다 update하는 방법
		- 빠르지만 왔다리 갔다리 함
	- Mini-Batch Gradient Descent
		- training set을 mini-batch로 나눠서 각 mini-batch별로 update
- learning rate를 적절히 정하는 것이 아무튼 중요함
	- Local minimum은 피하고, 그렇다고 너무 크면 발산할 가능성 존재함
	- 그래서 learning rate를 가변적으로 바꾸는 방법이 Learning rate decay
		- $\eta = \frac{\eta_{0}}{(1+\omega \times \text{epoch})}$
		- 위 식 말고도 여러 식들이 존재

Classification
- 여러 class에 대한 classification을 위해서 softmax함수를 사용한다. 
	- $\text{softmax}(y)_{i}=\frac{\exp(y_{i})}{\sum_{j=0}^n \exp(y_{i})}$
- 이때 Loss는 negative log likelihood를 사용함
- 그런데 Training set과 Testing set은 다르다. overfitting, underfitting문제가 발생할 수 있음
	- L2 Regularization
		- $C = C_{0}+\frac{\lambda}{2n}\sum_{w}w^2$
		- $\frac{\partial C}{\partial w}=\frac{\partial C_{0}}{\partial w}+\frac{\lambda}{n}w$
		- $w\to w-\eta \frac{\partial C_{0}}{\partial w}-\frac{\eta \lambda}{n}w=\left( 1-\frac{\eta \lambda}{n} \right)w-\eta \frac{\partial C_{0}}{\partial w}$
		- 이렇게 만듦으로써 현재 w의 영향을 줄이는 효과를 낸다. 
	- Dropout
		- 몇몇 neuron을 disable함(확률적으로)
