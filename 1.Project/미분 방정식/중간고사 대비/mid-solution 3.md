# 중간고사 대비 Test 3 해답

대응 시험지: `중간고사 대비 test 3.pdf`

## 1. Section 6.2, Problem 6

$$
F(s)=\frac{8s^2-6s+12}{s(s^2+4)}.
$$

부분분수로 분해하면

$$
F(s)=\frac3s+\frac{5s-6}{s^2+4}.
$$

따라서

$$
\boxed{
\mathcal{L}^{-1}\{F(s)\}
=3+5\cos(2t)-3\sin(2t)}.
$$

## 2. Section 3.5, Problem 15

$$
y''+2y'+5y=4e^{-t}\cos(2t),
\qquad y(0)=0,quad y'(0)=0.
$$

$y=e^{-t}u$로 치환하면

$$
y''+2y'+5y=e^{-t}(u''+4u).
$$

따라서 $u$는

$$
u''+4u=4\cos(2t)
$$

를 만족한다. 우변이 동차해와 공진하므로

$$
u=C_1\cos(2t)+C_2\sin(2t)+t\sin(2t).
$$

$y(0)=0$에서 $C_1=0$이고, $y'(0)=u'(0)-u(0)=0$에서 $C_2=0$이다. 따라서

$$
\boxed{y(t)=te^{-t}\sin(2t)}.
$$

## 3. Section 7.7, Problem 5

$$
\mathbf{x}'=
\begin{pmatrix}1&4\\1&-2\end{pmatrix}\mathbf{x}.
$$

고유값과 고유벡터는

$$
\lambda_1=2,\quad \mathbf{v}_1=\begin{pmatrix}4\\1\end{pmatrix},
\qquad
\lambda_2=-3,\quad \mathbf{v}_2=\begin{pmatrix}1\\-1\end{pmatrix}.
$$

따라서 한 기본행렬은

$$
\boxed{
X(t)=
\begin{pmatrix}
4e^{2t}&e^{-3t}\\
e^{2t}&-e^{-3t}
\end{pmatrix}}.
$$

$\Phi(0)=I$를 만족하는 기본행렬은 $\Phi(t)=X(t)X(0)^{-1}$이다. 여기서

$$
X(0)^{-1}=\frac15
\begin{pmatrix}1&1\\1&-4\end{pmatrix}.
$$

그러므로

$$
\boxed{
\Phi(t)=\frac15
\begin{pmatrix}
4e^{2t}+e^{-3t}&4e^{2t}-4e^{-3t}\\
e^{2t}-e^{-3t}&e^{2t}+4e^{-3t}
\end{pmatrix}}.
$$

실제로 $t=0$을 대입하면 $\Phi(0)=I$이다.

## 4. Section 2.5, Problem 18

Gompertz 방정식은

$$
\frac{dy}{dt}=ry\ln\frac{K}{y},
\qquad y(0)=y_0.
$$

### (a)

$$
z=\ln\frac{K}{y}
$$

로 두면

$$
z'=-\frac{y'}{y}=-rz.
$$

따라서

$$
z(t)=\ln\frac{K}{y_0}\,e^{-rt}.
$$

이를 $y$에 대해 풀면

$$
\boxed{
y(t)=K\exp\left[-\ln\left(\frac{K}{y_0}\right)e^{-rt}\right]
=K\left(\frac{y_0}{K}\right)^{e^{-rt}}}.
$$

### (b)

$r=0.71\ \text{year}^{-1}$, $K=80.5\times10^6$ kg, $y_0/K=0.25$이므로

$$
\frac{y(3)}K
=\exp[-(\ln4)e^{-0.71(3)}]
\approx0.848111.
$$

따라서

$$
\boxed{y(3)\approx6.8273\times10^7\ \text{kg}}
$$

즉 약 $68.27$ million kg이다.

### (c)

$y(\tau)=0.75K$를 대입하면

$$
\ln\frac43=(\ln4)e^{-0.71\tau}.
$$

그러므로

$$
\boxed{
\tau=\frac1{0.71}
\ln\left(\frac{\ln4}{\ln(4/3)}\right)
\approx2.215\ \text{years}}.
$$

## 5. Section 3.2, Problem 14

$W(f,g)=fg'-f'g$이고

$$
W(f,g)=2e^{6t},
\qquad f(t)=e^{3t}
$$

이므로

$$
e^{3t}g'-3e^{3t}g=2e^{6t}.
$$

$e^{6t}$로 나누면

$$
e^{-3t}(g'-3g)=2.
$$

왼쪽은 $(e^{-3t}g)'$이므로

$$
e^{-3t}g=2t+C.
$$

따라서

$$
\boxed{g(t)=e^{3t}(2t+C)}.
$$

$Ce^{3t}$는 $f$의 상수배이므로 이를 더해도 Wronskian은 변하지 않는다. 예를 들어 $g(t)=2te^{3t}$로 택할 수 있다.

## 6. Section 7.4, Problem 9

비동차 연립방정식을

$$
L[\mathbf{x}]
:=\mathbf{x}'-P(t)\mathbf{x}=\mathbf{g}(t)
$$

로 쓰자. $\mathbf{x}^{(p)}$가 한 특수해이고 $\mathbf{x}^{(c)}$가 대응하는 동차방정식의 임의의 해이면

$$
L[\mathbf{x}^{(p)}+\mathbf{x}^{(c)}]
=L[\mathbf{x}^{(p)}]+L[\mathbf{x}^{(c)}]
=\mathbf{g}+\mathbf{0}
=\mathbf{g}.
$$

따라서 $\mathbf{x}^{(p)}+\mathbf{x}^{(c)}$는 비동차방정식의 해이다.

반대로 $\mathbf{x}$가 비동차방정식의 임의의 해라면

$$
L[\mathbf{x}-\mathbf{x}^{(p)}]
=L[\mathbf{x}]-L[\mathbf{x}^{(p)}]
=\mathbf{g}-\mathbf{g}
=\mathbf{0}.
$$

즉 $\mathbf{x}-\mathbf{x}^{(p)}$는 동차방정식의 해 $\mathbf{x}^{(c)}$이고,

$$
\boxed{\mathbf{x}=\mathbf{x}^{(p)}+\mathbf{x}^{(c)}}.
$$

이로써 이 형태가 모든 해를 빠짐없이 나타냄을 보였다.

## 7. Section 2.8, Problem 14

$f_y=\partial f/\partial y$가 닫힌 직사각형 $D$에서 연속이라고 하자. 연속함수 $|f_y|$는 콤팩트 집합 $D$에서 최댓값을 가지므로

$$
K:=\max_{(t,y)\in D}|f_y(t,y)|<\infty
$$

로 둘 수 있다.

$t$를 고정하고 $(t,y_1),(t,y_2)\in D$를 택하자. $y$에 관한 평균값정리에 의해 $y_1$과 $y_2$ 사이의 어떤 $\xi$가 존재하여

$$
f(t,y_1)-f(t,y_2)=f_y(t,\xi)(y_1-y_2).
$$

따라서

$$
\boxed{
|f(t,y_1)-f(t,y_2)|
\le K|y_1-y_2|}.
$$

즉 $f$는 $D$에서 $y$에 관하여 Lipschitz 조건을 만족한다.

## 8. Section 3.4, Problem 7

$$
16y''-24y'+9y=0.
$$

특성방정식은

$$
16r^2-24r+9=(4r-3)^2=0.
$$

중근 $r=3/4$를 가지므로

$$
\boxed{y=(C_1+C_2t)e^{3t/4}}.
$$

## 9. Section 7.5, Problem 10

$$
\mathbf{x}'=
\begin{pmatrix}-2&1\\-5&4\end{pmatrix}\mathbf{x},
\qquad
\mathbf{x}(0)=\begin{pmatrix}2\\3\end{pmatrix}.
$$

고유값과 고유벡터는

$$
\lambda_1=3,\quad \mathbf{v}_1=\begin{pmatrix}1\\5\end{pmatrix},
\qquad
\lambda_2=-1,\quad \mathbf{v}_2=\begin{pmatrix}1\\1\end{pmatrix}.
$$

따라서

$$
\mathbf{x}(t)=c_1e^{3t}\begin{pmatrix}1\\5\end{pmatrix}
+c_2e^{-t}\begin{pmatrix}1\\1\end{pmatrix}.
$$

초기조건에서

$$
c_1+c_2=2,
\qquad 5c_1+c_2=3,
$$

이므로 $c_1=1/4$, $c_2=7/4$이다. 따라서

$$
\boxed{
\mathbf{x}(t)=
\frac14e^{3t}\begin{pmatrix}1\\5\end{pmatrix}
+\frac74e^{-t}\begin{pmatrix}1\\1\end{pmatrix}}.
$$

$t\to\infty$일 때 $e^{-t}$ 항은 사라지고 $e^{3t}$ 항이 지배하므로

$$
\mathbf{x}(t)\sim\frac14e^{3t}\begin{pmatrix}1\\5\end{pmatrix}.
$$

즉 해는 방향 $(1,5)^T$를 따라 크기가 무한히 증가한다.

## 10. Section 2.3, Problem 10

실내 온도를 $20^\circ\mathrm{C}$, 커피의 온도를 $T(t)$라 하자. Newton의 냉각법칙에 따라

$$
T'= -k(T-20),
$$

따라서

$$
T(t)=20+70e^{-kt}.
$$

$T(1)=85$를 대입하면

$$
65=70e^{-k}
\quad\Longrightarrow\quad
e^{-k}=\frac{13}{14},
\qquad k=\ln\frac{14}{13}.
$$

$T(t)=65$일 때

$$
45=70e^{-kt}
\quad\Longrightarrow\quad
e^{-kt}=\frac9{14}.
$$

따라서

$$
\boxed{
t=\frac{\ln(14/9)}{\ln(14/13)}
\approx5.962\ \text{min}}.
$$
