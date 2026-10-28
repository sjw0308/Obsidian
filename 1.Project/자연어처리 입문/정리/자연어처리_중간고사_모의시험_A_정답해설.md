# 자연어처리 입문 중간고사 모의시험 A 정답과 해설

문제지: [[자연어처리_중간고사_모의시험_A_문제.pdf]]

문제를 모두 푼 뒤 확인하세요. 총 7문항, 100점이며 풀이 과정에 따라 부분점수를 부여합니다.

## 1. 정규표현식 적용 — 15점

입력 문장:

```text
The cats saw a cat, then 12 dogs and 003 birds. The cat slept.
```

### (1) `[tT]he` — 3점

```text
The, the, The
```

처음의 `The`, `then` 앞부분의 `the`, 마지막 문장의 `The`가 매칭됩니다. 단어 경계 조건이 없으므로 `then` 안의 부분 문자열도 포함합니다.

### (2) `\b[tT]he\b` — 3점

```text
The, The
```

양쪽에 단어 경계가 있어야 합니다. `then` 안의 `the` 바로 뒤에는 단어 문자 `n`이 있으므로 제외됩니다.

### (3) `\d+` — 3점

```text
12, 003
```

숫자가 한 번 이상 이어진 문자열을 찾습니다. 정수로 변환하는 작업이 아니므로 `003`의 앞쪽 0을 유지합니다.

### (4) `cat(?=s)` — 3점

```text
cat
```

`cats` 안의 `cat`만 매칭됩니다. Positive lookahead는 바로 뒤의 `s`를 검사하지만 소비하지 않으므로 결과에 `s`는 포함되지 않습니다.

### (5) 숫자 치환 — 3점

검색 패턴은 `(\d+)`, 치환 문자열은 `<\1>`입니다.

```text
The cats saw a cat, then <12> dogs and <003> birds. The cat slept.
```

`\1`은 첫 번째 캡처 그룹의 매칭 결과입니다. 두 숫자 문자열을 각각 꺾쇠괄호로 감쌉니다.

## 2. BPE를 2회 수행하기 — 15점

### 초기 상태

| 단어 | 빈도 | 초기 분할 |
|---|---:|---|
| tall | 4 | `t a l l` |
| taller | 2 | `t a l l e r` |
| talk | 3 | `t a l k` |
| small | 1 | `s m a l l` |

초기 vocabulary:

```text
{t, a, l, e, r, k, s, m}
```

### (1) 회차별 선택 쌍과 빈도 — 8점

초기 인접 쌍의 빈도는 다음과 같습니다.

| 쌍 | 빈도 |
|---|---:|
| `(t, a)` | 4 + 2 + 3 = 9 |
| `(a, l)` | 4 + 2 + 3 + 1 = 10 |
| `(l, l)` | 4 + 2 + 1 = 7 |
| `(l, e)` | 2 |
| `(e, r)` | 2 |
| `(l, k)` | 3 |
| `(s, m)` | 1 |
| `(m, a)` | 1 |

**1회: `(a, l)`을 `al`로 병합합니다. 빈도는 10입니다.**

선택한 쌍을 모든 단어에 적용합니다.

```text
tall   → t al l
taller → t al l e r
talk   → t al k
small  → s m al l
```

병합 후의 인접 쌍 빈도를 다시 계산합니다.

| 쌍 | 빈도 |
|---|---:|
| `(t, al)` | 9 |
| `(al, l)` | 7 |
| `(al, k)` | 3 |
| `(l, e)` | 2 |
| `(e, r)` | 2 |
| `(s, m)` | 1 |
| `(m, al)` | 1 |

**2회: `(t, al)`을 `tal`로 병합합니다. 빈도는 9입니다.**

회차별로 선택 쌍과 빈도를 맞히면 각 4점입니다.

### (2) 2회 뒤 분할 — 4점

| 단어 | 최종 분할 |
|---|---|
| tall | `tal l` |
| taller | `tal l e r` |
| talk | `tal k` |
| small | `s m al l` |

단어별 1점입니다.

### (3) 최종 vocabulary — 3점

```text
{t, a, l, e, r, k, s, m, al, tal}
```

Vocabulary 크기는 **10**입니다. 새 병합 토큰 `al`, `tal`을 추가하되 기존 문자 토큰도 유지합니다. 현재 단어 분할에 사용되지 않는 토큰이 vocabulary에서 사라지는 것은 아닙니다.

## 3. Bigram 확률 계산 — 15점

### (1) 조건부확률 — 각 2점, 총 8점

$$
P(we\mid\texttt{<s>})=\frac34
$$

네 문장 중 세 문장이 `we`로 시작합니다.

$$
P(study\mid we)=\frac23
$$

`we` 다음에 `study`가 2회, `like`가 1회 등장합니다.

$$
P(nlp\mid study)=\frac23
$$

`study` 다음에 `nlp`가 2회, `math`가 1회 등장합니다.

$$
P(\texttt{</s>}\mid nlp)=\frac33=1
$$

`nlp`가 등장한 세 번 모두 바로 뒤에 종료 토큰이 있습니다. 분모는 전체 corpus의 단어 수가 아니라 **조건 문맥 뒤의 전이 횟수**입니다.

### (2) 문장 확률 — 5점

시작·종료 전이를 모두 포함하면 다음과 같습니다.

$$
\begin{aligned}
P(we\ study\ nlp\ \texttt{</s>}\mid\texttt{<s>})
&=P(we\mid\texttt{<s>})P(study\mid we)P(nlp\mid study)P(\texttt{</s>}\mid nlp)\\
&=\frac34\cdot\frac23\cdot\frac23\cdot1\\
&=\boxed{\frac13}
\end{aligned}
$$

전이의 곱을 정확히 구성하면 3점, 최종 계산 결과는 2점입니다.

### (3) `we like math`의 확률 — 2점

**정답은 0입니다.**

$$
P(math\mid like)=\frac01=0
$$

학습 corpus에 `like math`라는 전이가 없습니다. Smoothing을 하지 않으므로 해당 조건부확률이 0이고, 전체 문장 확률도 0이 됩니다. 값 1점, 이유 1점입니다.

## 4. Add-1 Naive Bayes 문서 분류 — 22점

테스트 문서는 `bright mystery story dull`입니다. 문제 조건에 따라 vocabulary 밖 단어 `mystery`를 무시하므로 계산에 사용할 단어는 `bright`, `story`, `dull`입니다.

### (1) Prior — 4점

각 클래스의 학습 문서가 2개, 전체 문서가 4개입니다.

$$
P(+)=\frac24=\frac12,\qquad P(-)=\frac24=\frac12
$$

각 2점입니다. Prior는 단어 수가 아니라 **문서 수**로 계산합니다.

### (2) Vocabulary와 token 수 — 4점

$$
V=\{bright,fun,story,dull,slow\},\qquad |V|=5
$$

- Positive token 수: `bright fun`의 2개 + `bright bright story`의 3개 = **5개**.
- Negative token 수: `dull slow`의 2개 + `dull story slow`의 3개 = **5개**.

따라서 $N_+=N_-=5$입니다. Vocabulary와 크기는 각 1점, 클래스별 token 수는 각 1점입니다.

### (3) 클래스별 likelihood — 6점

Add-1 smoothing을 적용합니다.

$$
P(w\mid c)=\frac{C(w,c)+1}{N_c+|V|}
$$

모든 클래스에서 분모는 $5+5=10$입니다.

| 단어 | + count | − count | $P(w\mid+)$ | $P(w\mid-)$ |
|---|---:|---:|---:|---:|
| bright | 3 | 0 | 4/10 | 1/10 |
| story | 1 | 1 | 2/10 | 2/10 |
| dull | 0 | 2 | 1/10 | 3/10 |

Likelihood 여섯 개 각각 1점입니다. `dull`은 positive 클래스에서 관측되지 않았지만 전체 vocabulary에는 있는 단어이므로 무시하지 않고 smoothing을 적용합니다.

### (4) 클래스별 비정규화 점수 — 6점

$$
\begin{aligned}
s(+)&=P(+)P(bright\mid+)P(story\mid+)P(dull\mid+)\\
&=\frac12\cdot\frac4{10}\cdot\frac2{10}\cdot\frac1{10}\\
&=\boxed{0.004=\frac1{250}}
\end{aligned}
$$

$$
\begin{aligned}
s(-)&=P(-)P(bright\mid-)P(story\mid-)P(dull\mid-)\\
&=\frac12\cdot\frac1{10}\cdot\frac2{10}\cdot\frac3{10}\\
&=\boxed{0.003=\frac3{1000}}
\end{aligned}
$$

클래스당 식 2점, 계산 결과 1점입니다.

### (5) 최종 클래스 — 2점

$$
s(+)>s(-)\quad\Longrightarrow\quad\boxed{+\text{ 클래스}}
$$

위 점수는 정규화된 posterior 자체가 아닙니다. 참고로 정규화하면 $P(+\mid d)=0.004/(0.004+0.003)=4/7$입니다. 이 문제에서는 정규화까지 요구하지 않습니다.

## 5. Gradient descent 표 완성 — 15점

모델과 손실은 다음과 같습니다.

$$
y=2w_0+w_1,\qquad \mathcal L=(y-5)^2,\qquad \eta=0.05
$$

### (1) Gradient — 3점

$$
\frac{\partial\mathcal L}{\partial w_0}
=\frac{\partial\mathcal L}{\partial y}\frac{\partial y}{\partial w_0}
=2(y-5)\cdot2=\boxed{4(y-5)}
$$

$$
\frac{\partial\mathcal L}{\partial w_1}
=2(y-5)\cdot1=\boxed{2(y-5)}
$$

연쇄법칙 적용 1점, 각 식 1점입니다.

### (2) 완성된 표 — 12점

| 완료한 업데이트 횟수 $k$ | $w_0$ | $w_1$ | $y$ | $\mathcal L$ |
|---:|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 | 25 |
| 1 | 1 | 0.5 | 2.5 | 6.25 |
| 2 | 1.5 | 0.75 | 3.75 | 1.5625 |
| 3 | 1.75 | 0.875 | 4.375 | 0.390625 |

문제지의 빈칸 12개 각각 1점입니다.

업데이트 식은 $w_i^{new}=w_i-\eta\,\partial\mathcal L/\partial w_i$입니다.

| 현재 행 | 현재 gradient $(\partial\mathcal L/\partial w_0,\partial\mathcal L/\partial w_1)$ | 다음 가중치 $(w_0,w_1)$ |
|---|---|---|
| 0행 | $(-20,-10)$ | $(0,0)-0.05(-20,-10)=(1,0.5)$ |
| 1행 | $(-10,-5)$ | $(1,0.5)-0.05(-10,-5)=(1.5,0.75)$ |
| 2행 | $(-5,-2.5)$ | $(1.5,0.75)-0.05(-5,-2.5)=(1.75,0.875)$ |

두 가중치의 gradient는 **같은 현재 행**에서 구한 뒤 함께 갱신합니다. 각 행의 출력과 손실은 그 행의 가중치로 계산하며, 출력 $y$ 자체가 아니라 손실 $\mathcal L$의 gradient를 사용합니다.

## 6. Backpropagation과 가중치 업데이트 — 15점

### (1) 순전파 — 각 1점, 총 5점

입력은 $x_0=2$, $x_1=1$이고 $a=1.5$입니다.

| 값 | 계산 | 결과 |
|---|---|---:|
| $h_{10}$ | $ax_0+x_1=1.5\times2+1$ | 4 |
| $h_{11}$ | $x_0-x_1=2-1$ | 1 |
| $h_{20}$ | $3h_{10}+h_{11}=3\times4+1$ | 13 |
| $h_{21}$ | $2h_{10}+2h_{11}=2\times4+2\times1$ | 10 |
| $y$ | $h_{20}-h_{21}=13-10$ | 3 |

### (2) 출력에 대한 미분 — 4점

$$
\frac{\partial h_{10}}{\partial a}=x_0=2
$$

$a$는 $h_{10}$을 통해 두 경로로 출력에 영향을 줍니다. $h_{11}$은 $a$에 의존하지 않습니다.

**경로 1:** $a\to h_{10}\to h_{20}\to y$

$$
\frac{\partial h_{10}}{\partial a}
\frac{\partial h_{20}}{\partial h_{10}}
\frac{\partial y}{\partial h_{20}}
=2\cdot3\cdot1=6
$$

**경로 2:** $a\to h_{10}\to h_{21}\to y$

$$
\frac{\partial h_{10}}{\partial a}
\frac{\partial h_{21}}{\partial h_{10}}
\frac{\partial y}{\partial h_{21}}
=2\cdot2\cdot(-1)=-4
$$

두 경로의 기여를 합산합니다.

$$
\boxed{\frac{\partial y}{\partial a}=6+(-4)=2}
$$

각 경로의 기여 1점씩, 합산한 미분 2점입니다. 두 번째 경로는 출력식 $y=h_{20}-h_{21}$ 때문에 음수가 됩니다.

### (3) 손실에 대한 미분 — 3점

$$
\mathcal L=(y-2)^2
$$

$$
\frac{\partial\mathcal L}{\partial a}
=\frac{\partial\mathcal L}{\partial y}\frac{\partial y}{\partial a}
=2(y-2)\cdot2
=2(3-2)\cdot2
=\boxed{4}
$$

손실 도함수 1점, 연쇄법칙의 곱 1점, 값 1점입니다.

### (4) 가중치 업데이트 — 3점

$$
a^{new}=a-\eta\frac{\partial\mathcal L}{\partial a}
=1.5-0.1\cdot4
=\boxed{1.1}
$$

손실 gradient 대입 1점, 학습률 반영 1점, 최종 값 1점입니다. $\partial y/\partial a=2$만 사용하면 문제에서 요구하는 손실 최소화 업데이트와 달라집니다.

## 7. 다중 분류 손실함수 빈칸 — 3점

$$
\boxed{A=-1},\qquad \boxed{B=p_2=0.60}
$$

A는 1점, B는 2점입니다. 클래스 인덱스는 1, 2, 3으로 셉니다.

정답 one-hot 벡터가 $(0,1,0)$이므로 두 번째 클래스가 정답입니다.

$$
\mathcal L=-\sum_{k=1}^{3}t_k\log p_k
=-(0\log0.15+1\log0.60+0\log0.25)
=\boxed{-\log0.60}
$$

정답 클래스 확률의 로그를 사용합니다. 가장 작은 확률이나 세 확률의 합을 넣지 않습니다. 문제 조건에 따라 로그값을 수치로 계산할 필요는 없습니다.

## 채점 후 확인

- 빈도·분모 오류: BPE의 단어 빈도 반영, N-gram의 조건 문맥, NB의 문서 수·token 수·type 수를 구분합니다.
- Gradient·부호 오류: 같은 현재 가중치에서 미분하고, 역전파의 모든 경로와 출력식의 음수를 반영합니다.
- 정규표현식 오류: 단어 경계와 lookahead의 검사·소비 차이를 확인합니다.

풀이 방식은 맞고 단순 산술 오류만 있다면 각 소문항에 표시된 과정 점수를 반영하여 채점할 수 있습니다.
