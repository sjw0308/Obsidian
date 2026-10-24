# 중간고사 대비 Test 2 해답

대응 시험지: `중간고사 대비 test 2.pdf`

## 1. Section 3.3, Problem 9

$$
y''+6y'+9.25y=0.
$$

특성방정식은

$$
r^2+6r+\frac{37}{4}=0,
$$

이고 근은

$$
r=-3\pm\frac12i
$$

이다. 따라서

$$
\boxed{
y=e^{-3t}\left(C_1\cos\frac{t}{2}+C_2\sin\frac{t}{2}\right)}.
$$

## 2. Section 7.1, Problem 4

$$
u''+2u'+4u=2\cos(3t),
\qquad u(0)=1,quad u'(0)=-2.
$$

$x_1=u$, $x_2=u'$로 놓으면

$$
x_1'=x_2,
\qquad
x_2'=-4x_1-2x_2+2\cos(3t).
$$

따라서 요구되는 일차 연립 초기값 문제는

$$
\boxed{
\begin{aligned}
\mathbf{x}'&=
\begin{pmatrix}0&1\\-4&-2\end{pmatrix}\mathbf{x}
+\begin{pmatrix}0\\2\cos(3t)\end{pmatrix},\\
\mathbf{x}(0)&=\begin{pmatrix}1\\-2\end{pmatrix}.
\end{aligned}}
$$

## 3. Section 2.6, Problem 9

$$
(2x-y)+(2y-x)y'=0,
\qquad y(1)=4.
$$

$M(x,y)=2x-y$, $N(x,y)=2y-x$라 두면

$$
M_y=-1=N_x,
$$

이므로 완전미분방정식이다. $\psi_x=M$을 적분하면

$$
\psi=x^2-xy+h(y).
$$

$\psi_y=N$을 이용하면 $h'(y)=2y$이므로

$$
x^2-xy+y^2=C.
$$

초기조건으로부터 $C=1-4+16=13$이므로

$$
x^2-xy+y^2=13.
$$

$y$에 관한 이차방정식으로 풀고 $y(1)=4$인 가지를 택하면

$$
\boxed{y(x)=\frac{x+\sqrt{52-3x^2}}{2}}.
$$

제곱근이 양수이고 $2y-x\ne0$이어야 하므로 초기점 $x=1$을 포함하는 최대 유효구간은

$$
\boxed{-\sqrt{\frac{52}{3}}<x<\sqrt{\frac{52}{3}}}
\approx\boxed{-4.163<x<4.163}.
$$

## 4. Section 6.2, Problem 10

$$
y''-2y'+2y=0,
\qquad y(0)=1,quad y'(0)=1.
$$

$Y(s)=\mathcal{L}\{y(t)\}$라 하면

$$
(s^2Y-s-1)-2(sY-1)+2Y=0.
$$

따라서

$$
Y(s)=\frac{s-1}{(s-1)^2+1}.
$$

이동정리를 적용하면

$$
\boxed{y(t)=e^t\cos t}.
$$

## 5. Section 7.8, Problem 2

$$
\mathbf{x}'=A\mathbf{x},
\qquad
A=\begin{pmatrix}
1&1&1\\
2&1&-1\\
0&-1&1
\end{pmatrix}.
$$

$a=1-\lambda$라 놓으면

$$
\det(A-\lambda I)=a^3-3a-2=(a-2)(a+1)^2.
$$

따라서 고유값은 $\lambda=-1$과 중복도 2의 $\lambda=2$이다.

$\lambda=-1$에 대해서는

$$
\mathbf{u}=\begin{pmatrix}-3\\4\\2\end{pmatrix},
$$

$\lambda=2$에 대해서는 고유벡터

$$
\mathbf{v}=\begin{pmatrix}0\\1\\-1\end{pmatrix}
$$

하나만 독립적으로 얻어진다. 일반화 고유벡터 $\mathbf{w}$를

$$
(A-2I)\mathbf{w}=\mathbf{v}
$$

가 되도록 택하면 $\mathbf{w}=(1,1,0)^T$로 둘 수 있다. 그러므로 일반해는

$$
\boxed{
\mathbf{x}(t)=
C_1e^{-t}\begin{pmatrix}-3\\4\\2\end{pmatrix}
+C_2e^{2t}\begin{pmatrix}0\\1\\-1\end{pmatrix}
+C_3e^{2t}
\left[
\begin{pmatrix}1\\1\\0\end{pmatrix}
+t\begin{pmatrix}0\\1\\-1\end{pmatrix}
\right]}.
$$

## 6. Section 2.2, Problem 5

$$
\frac{dy}{dx}=\frac{3x-e^{-x}}{2y+e^y}.
$$

변수를 분리하면

$$
(2y+e^y)\,dy=(3x-e^{-x})\,dx.
$$

양변을 적분하여

$$
\boxed{y^2+e^y=\frac32x^2+e^{-x}+C}.
$$

## 7. Section 3.6, Problem 4

$$
y''+y=2\tan t,
\qquad 0<t<\frac{\pi}{2}.
$$

동차해는

$$
y_c=C_1\cos t+C_2\sin t.
$$

$y_1=\cos t$, $y_2=\sin t$, $W=1$을 사용하여 매개변수변화법을 적용하면

$$
y_p=-\cos t\int 2\sin t\tan t\,dt
+\sin t\int 2\cos t\tan t\,dt.
$$

각 적분은

$$
\int2\sin t\tan t\,dt
=2\ln(\sec t+\tan t)-2\sin t,
$$

$$
\int2\cos t\tan t\,dt=-2\cos t.
$$

이므로 교차항이 소거되어

$$
y_p=-2\cos t\ln(\sec t+\tan t).
$$

따라서

$$
\boxed{
y=C_1\cos t+C_2\sin t
-2\cos t\ln(\sec t+\tan t)},
\qquad 0<t<\frac{\pi}{2}.
$$

## 8. Section 7.3, Problem 17

$$
A=\begin{pmatrix}5&3\\-1&1\end{pmatrix}.
$$

특성방정식은

$$
\det(A-\lambda I)
=(5-\lambda)(1-\lambda)+3
=(\lambda-2)(\lambda-4)=0.
$$

따라서

$$
\boxed{
\lambda=2:\ \mathbf{v}=c\begin{pmatrix}1\\-1\end{pmatrix},\quad c\ne0}
$$

및

$$
\boxed{
\lambda=4:\ \mathbf{v}=c\begin{pmatrix}-3\\1\end{pmatrix},\quad c\ne0}
$$

이다.

## 9. Section 2.1, Problem 13

$$
ty'+2y=t^2-t+1,
\qquad y(1)=2,quad t>0.
$$

$t$로 나누면

$$
y'+\frac2t y=t-1+\frac1t.
$$

적분인자는 $\mu(t)=t^2$이므로

$$
(t^2y)'=t^3-t^2+t.
$$

적분하면

$$
y=\frac{t^2}{4}-\frac{t}{3}+\frac12+\frac{C}{t^2}.
$$

$y(1)=2$를 대입하면 $C=19/12$이다. 따라서

$$
\boxed{
y(t)=\frac{t^2}{4}-\frac{t}{3}+\frac12+\frac{19}{12t^2}},
\qquad t>0.
$$

## 10. Section 3.1, Problem 6

$$
y''-4y'-4y=0.
$$

특성방정식

$$
r^2-4r-4=0
$$

의 근은 $r=2\pm2\sqrt2$이다. 그러므로

$$
\boxed{
y=C_1e^{(2+2\sqrt2)t}+C_2e^{(2-2\sqrt2)t}}.
$$
