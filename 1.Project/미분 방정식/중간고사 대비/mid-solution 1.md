# 중간고사 대비 Test 1 해답

대응 시험지: `중간고사 대비 test 1.pdf`

## 1. Section 7.6, Problem 5

주어진 초기값 문제는

$$
\mathbf{x}'=
\begin{pmatrix}-3&2\\-1&-1\end{pmatrix}\mathbf{x},
\qquad
\mathbf{x}(0)=\begin{pmatrix}1\\2\end{pmatrix}
$$

이다. 계수행렬을 $A$라 하면

$$
\det(A-\lambda I)=\lambda^2+4\lambda+5=0,
$$

따라서 고유값은 $\lambda=-2\pm i$이다. $\lambda=-2+i$에 대한 고유벡터를

$$
\begin{pmatrix}2\\1+i\end{pmatrix}
=
\begin{pmatrix}2\\1\end{pmatrix}
+i\begin{pmatrix}0\\1\end{pmatrix}
$$

로 잡을 수 있다. 실수해의 형태에 초기조건을 대입하면

$$
\boxed{
\mathbf{x}(t)=e^{-2t}
\begin{pmatrix}
\cos t+3\sin t\\
2\cos t+\sin t
\end{pmatrix}}
$$

를 얻는다. 진동항의 진폭이 $e^{-2t}$로 감소하므로

$$
\boxed{\mathbf{x}(t)\to\mathbf{0}\quad(t\to\infty)}.
$$

즉, 원점은 안정 나선점이다.

## 2. Section 2.3, Problem 1

$Q(t)$를 탱크 안 염료의 양(g)이라 하자. 탱크의 부피는 계속 $150$ L이고, 유입수에는 염료가 없으므로

$$
Q'(t)=0-2\frac{Q(t)}{150}=-\frac{Q(t)}{75},
\qquad Q(0)=150.
$$

따라서

$$
Q(t)=150e^{-t/75},
\qquad C(t)=\frac{Q(t)}{150}=e^{-t/75}\ \text{g/L}.
$$

농도가 처음 농도의 $1\%$가 되는 때에는

$$
e^{-t/75}=0.01.
$$

그러므로

$$
\boxed{t=75\ln 100\approx345.39\ \text{min}\approx5.76\ \text{h}}.
$$

## 3. Section 3.5, Problem 7

$$
y''+y=4\sin(2t)+t\cos(2t).
$$

동차방정식의 해는

$$
y_c=C_1\cos t+C_2\sin t.
$$

$4\sin(2t)$에 대해서는 $y_{p1}=A\sin(2t)$로 두면 $-3A=4$이므로

$$
y_{p1}=-\frac43\sin(2t).
$$

$t\cos(2t)$에 대해서

$$
y_{p2}=(at+b)\cos(2t)+(ct+d)\sin(2t)
$$

로 두고 계수를 비교하면

$$
a=-\frac13,\qquad b=0,\qquad c=0,\qquad d=\frac49.
$$

따라서

$$
\boxed{
y=C_1\cos t+C_2\sin t- \frac{t}{3}\cos(2t)-\frac89\sin(2t)}.
$$

## 4. Section 6.1, Problem 14

먼저

$$
F(s)=\mathcal{L}\{\cos(at)\}=\int_0^\infty e^{-st}\cos(at)\,dt
=\frac{s}{s^2+a^2},\qquad s>0.
$$

적분을 매개변수 $s$에 대해 두 번 미분하면

$$
F''(s)=\int_0^\infty t^2e^{-st}\cos(at)\,dt
=\mathcal{L}\{t^2\cos(at)\}.
$$

따라서

$$
\boxed{
\mathcal{L}\{t^2\cos(at)\}
=\frac{2s(s^2-3a^2)}{(s^2+a^2)^3}}.
$$

## 5. Section 2.4, Problem 24

$$
t^2y'+2ty-y^3=0,qquad t>0
$$

를 표준형으로 쓰면

$$
y'+\frac2t y=\frac1{t^2}y^3.
$$

Bernoulli 치환 $v=y^{-2}$를 사용하면

$$
v'-\frac4t v=-\frac2{t^2}.
$$

적분인자는 $t^{-4}$이므로

$$
(t^{-4}v)'=-2t^{-6}.
$$

적분하여

$$
t^{-4}v=\frac25t^{-5}+C,
\qquad
v=\frac{2}{5t}+Ct^4.
$$

따라서 영이 아닌 해는

$$
\boxed{y(t)=\pm\left(\frac{2}{5t}+Ct^4\right)^{-1/2}},
$$

이고, 치환 과정에서 제외된 해 $\boxed{y(t)=0}$도 있다. 영이 아닌 해는 괄호 안이 양수인 구간에서 정의된다.

## 6. Section 7.2, Problem 12

$$
A=
\begin{pmatrix}
1&2&1\\
-2&1&8\\
-1&-1&1
\end{pmatrix}.
$$

행렬식은

$$
\begin{aligned}
\det A
&=1(1+8)-2(-2+8)+(2+1)\\
&=9-12+3=0.
\end{aligned}
$$

또한 열벡터를 $\mathbf{c}_1,\mathbf{c}_2,\mathbf{c}_3$라 하면

$$
\mathbf{c}_3=-3\mathbf{c}_1+2\mathbf{c}_2,
$$

이므로 열들이 선형종속이다. 따라서

$$
\boxed{A\text{는 특이행렬이며 역행렬이 존재하지 않는다.}}
$$

## 7. Section 3.4, Problem 18

$$
t^2y''-4ty'+6y=0,qquad t>0,qquad y_1=t^3.
$$

표준형의 $y'$ 계수는 $P(t)=-4/t$이다. 감차공식에 의해

$$
\begin{aligned}
y_2
&=y_1\int\frac{e^{-\int P(t)\,dt}}{y_1^2}\,dt\\
&=t^3\int\frac{t^4}{t^6}\,dt
=t^3\int t^{-2}\,dt
=-t^2.
\end{aligned}
$$

상수배는 무관하므로 $y_2=t^2$로 잡을 수 있다. 따라서

$$
\boxed{y=C_1t^3+C_2t^2}.
$$

## 8. Section 2.7, Problem 1

$$
y'=2+t-y,qquad y(0)=1.
$$

Euler 공식은

$$
y_{n+1}=y_n+h(2+t_n-y_n)
$$

이다. 그런데 $y_n=1+t_n$이면

$$
y_{n+1}=1+t_n+h=1+t_{n+1}.
$$

초기값이 $y_0=1=1+t_0$이므로 모든 격자점에서 이 관계가 성립한다.

| $t$ | $h=0.1$ | $h=0.05$ | $h=0.025$ | 정확해 |
|---:|---:|---:|---:|---:|
| 0.1 | 1.1 | 1.1 | 1.1 | 1.1 |
| 0.2 | 1.2 | 1.2 | 1.2 | 1.2 |
| 0.3 | 1.3 | 1.3 | 1.3 | 1.3 |
| 0.4 | 1.4 | 1.4 | 1.4 | 1.4 |

실제로 미분방정식을 풀면

$$
y'+y=t+2
\quad\Longrightarrow\quad
y=t+1+Ce^{-t}.
$$

$y(0)=1$에서 $C=0$이므로 $\boxed{y=t+1}$이다. 따라서 이 문제에서는 세 간격 모두 제시된 점에서 오차가 정확히 $0$이다.

## 9. Section 7.9, Problem 3

$$
\mathbf{x}'=
\begin{pmatrix}1&4\\1&-2\end{pmatrix}\mathbf{x}
+\begin{pmatrix}e^{-2t}\\-2e^t\end{pmatrix}.
$$

동차계의 고유값과 고유벡터는

$$
\lambda_1=2,\quad \mathbf{v}_1=\begin{pmatrix}4\\1\end{pmatrix},
\qquad
\lambda_2=-3,\quad \mathbf{v}_2=\begin{pmatrix}1\\-1\end{pmatrix}.
$$

따라서

$$
\mathbf{x}_c
=C_1e^{2t}\begin{pmatrix}4\\1\end{pmatrix}
+C_2e^{-3t}\begin{pmatrix}1\\-1\end{pmatrix}.
$$

강제항을 두 부분으로 나누어

$$
\mathbf{x}_p=\mathbf{q}e^{-2t}+\mathbf{r}e^t
$$

로 두면

$$
(-2I-A)\mathbf{q}=\begin{pmatrix}1\\0\end{pmatrix},
\qquad
(I-A)\mathbf{r}=\begin{pmatrix}0\\-2\end{pmatrix}.
$$

이를 풀어

$$
\mathbf{q}=\begin{pmatrix}0\\-1/4\end{pmatrix},
\qquad
\mathbf{r}=\begin{pmatrix}2\\0\end{pmatrix}
$$

를 얻는다. 그러므로 일반해는

$$
\boxed{
\mathbf{x}(t)=
C_1e^{2t}\begin{pmatrix}4\\1\end{pmatrix}
+C_2e^{-3t}\begin{pmatrix}1\\-1\end{pmatrix}
+\begin{pmatrix}2e^t\\-\frac14e^{-2t}\end{pmatrix}}.
$$

## 10. Section 3.2, Problem 7

$$
t(t-4)y''+3ty'+5y=2,
\qquad y(3)=0,quad y'(3)=-1.
$$

$y''$의 계수로 나누면

$$
y''+\frac{3}{t-4}y'+\frac{5}{t(t-4)}y
=\frac{2}{t(t-4)}.
$$

계수들이 불연속이 되는 점은 $t=0,4$이다. 초기점 $t_0=3$을 포함하면서 두 특이점을 지나지 않는 가장 긴 열린구간은

$$
\boxed{(0,4)}.
$$
