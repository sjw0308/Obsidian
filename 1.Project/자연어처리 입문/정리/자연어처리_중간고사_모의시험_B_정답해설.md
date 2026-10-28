# 자연어처리 입문 중간고사 모의시험 B 정답과 해설

문제지: [[자연어처리_중간고사_모의시험_B_문제.pdf]]

총 8문항, 120점입니다. 1~7번은 100점, 벡터 의미론·임베딩 8번은 20점입니다. 문제를 모두 푼 뒤 확인하세요.

## 1. 정규표현식 — 15점

입력 문장:

```text
The fox saw other foxes, then 24 birds and 007 fish. The fox hid.
```

| 소문항 | 정답 | 배점 |
|---|---|---:|
| (1) `[tT]he` | `The, the, the, The` | 3점 |
| (2) `\b[tT]he\b` | `The, The` | 3점 |
| (3) `\d+` | `24, 007` | 3점 |
| (4) `fox(?=es)` | `fox` | 3점 |

(1)은 처음의 `The`, `other` 안의 `the`, `then`의 `the`, 마지막의 `The`를 찾습니다. 매칭 결과에 패턴 밖의 `r`, `n`을 넣지 않습니다. 결과 네 개에 각 0.75점입니다.

(2)는 독립된 단어 `The`만 찾아 두 번 적습니다. 각 출현에 1.5점입니다.

(3)은 숫자 문자열을 찾으므로 `007`을 `7`로 바꾸지 않습니다. 각 1.5점입니다.

(4)는 `foxes`의 `fox`만 찾습니다. Lookahead의 `es`는 결과에 포함하지 않습니다.

(5) 치환 결과는 다음과 같습니다. (3점)

```text
The fox saw other foxes, then [24] birds and [007] fish. The fox hid.
```

현재 매칭마다 그룹 1이 달라지므로 `24 → [24]`, `007 → [007]`입니다. 숫자 두 곳의 치환에 각 1점, 나머지 문장 보존에 1점입니다.

## 2. BPE — 15점

### (1) 병합 쌍과 빈도 — 8점

초기 주요 쌍 빈도는 `(l,a)=14`, `(a,y)=12`, `(p,l)=10`, `(c,l)=4`, `(y,e)=3`, `(e,r)=3`, `(a,i)=2`, `(i,n)=2`입니다.

$$
C(l,a)=5+3+2+4=14
$$

따라서 첫 병합은 **`(l,a) → la`, 빈도 14**입니다.

```text
play   → p la y
player → p la y e r
plain  → p la i n
clay   → c la y
```

이후 `(la,y)`의 빈도는 $5+3+4=12$이고 `(p,la)`의 10보다 큽니다. 두 번째 병합은 **`(la,y) → lay`, 빈도 12**입니다.

회차별 선택 쌍 2점, 빈도 2점입니다. 단어 종류 수 대신 실제 단어 빈도를 반영합니다.

### (2) 최종 분할 — 4점

| 단어 | 최종 분할 |
|---|---|
| play | `p lay` |
| player | `p lay e r` |
| plain | `p la i n` |
| clay | `c lay` |

단어별 1점입니다.

### (3) Vocabulary — 3점

```text
{p, l, a, y, e, r, i, n, c, la, lay}
```

초기 문자 9개를 유지하고 병합 토큰 2개를 추가하므로 크기는 11입니다. 나열 순서는 상관없습니다.

## 3. Bigram — 15점

### (1) 조건부확률 — 8점

$$
P(students\mid\texttt{<s>})=\frac35,\qquad
P(read\mid students)=\frac23
$$

$$
P(books\mid read)=\frac23,\qquad
P(\texttt{</s>}\mid books)=\frac22=1
$$

각 2점입니다. `read` 다음에는 `books`가 2회, `papers`가 1회 등장합니다.

### (2) 문장 확률 — 5점

$$
\begin{aligned}
P(students\ read\ books\ \texttt{</s>}\mid\texttt{<s>})
&=\frac35\cdot\frac23\cdot\frac23\cdot1\\
&=\boxed{\frac4{15}}
\end{aligned}
$$

시작·종료를 포함한 전이의 곱 3점, 최종 값 2점입니다.

### (3) `students write books` — 2점

**문장 확률은 0입니다.** 학습 문장에서 `write books`는 관측되지 않았습니다.

$$
P(books\mid write)=\frac{C(write,books)}{C(write)}=\frac02=0
$$

`books` 자체는 2회 등장하므로 $C(books)=0$이라는 설명은 틀립니다. 값 1점, 전이 count를 이용한 이유 1점입니다.

## 4. Naive Bayes — 22점

### (1) Prior — 4점

$$
P(+)=\frac35,\qquad P(-)=\frac25
$$

각 2점입니다. 클래스별 학습 문서가 각각 3개, 2개입니다.

### (2) Vocabulary와 token 수 — 4점

$$
V=\{clear,useful,guide,confusing,dull\},\quad |V|=5
$$

$$
N_+=2+3+2=7,\qquad N_-=2+2=4
$$

Vocabulary, 크기, 두 token 수에 각 1점입니다. `fresh`는 학습 vocabulary 밖이므로 문제 조건에 따라 무시합니다.

### (3) Likelihood — 6점

$$
P(w\mid c)=\frac{C(w,c)+1}{N_c+|V|}
$$

| 단어 | + count | − count | $P(w\mid+)$ | $P(w\mid-)$ |
|---|---:|---:|---:|---:|
| clear | 3 | 0 | 4/12 | 1/9 |
| guide | 2 | 1 | 3/12 | 2/9 |
| dull | 0 | 2 | 1/12 | 3/9 |

확률당 1점입니다. 클래스별 token 수가 다르므로 smoothing 분모도 다릅니다.

### (4) 점수 — 6점

$$
s(+)=\frac35\cdot\frac4{12}\cdot\frac3{12}\cdot\frac1{12}
=\boxed{\frac1{240}}\approx0.0041667
$$

$$
s(-)=\frac25\cdot\frac19\cdot\frac29\cdot\frac39
=\boxed{\frac4{1215}}\approx0.0032922
$$

클래스당 식 2점, 값 1점입니다. 분수나 충분한 자릿수의 소수 답을 인정합니다.

### (5) 분류 — 2점

$s(+)>s(-)$이므로 **+ 클래스**입니다. 위 점수는 비정규화 값이며 posterior 정규화는 요구하지 않습니다.

## 5. Gradient descent — 15점

### (1) 미분 — 3점

$$
y=w_0+3w_1,\qquad \mathcal L=(y-8)^2
$$

$$
\frac{\partial\mathcal L}{\partial w_0}=\boxed{2(y-8)},\qquad
\frac{\partial\mathcal L}{\partial w_1}=\boxed{6(y-8)}
$$

연쇄법칙 적용 1점, 각 식 1점입니다.

### (2) 표 — 12점

| $k$ | $w_0$ | $w_1$ | $y$ | $\mathcal L$ |
|---:|---:|---:|---:|---:|
| 0 | 0 | 0 | 0 | 64 |
| 1 | 0.4 | 1.2 | 4 | 16 |
| 2 | 0.6 | 1.8 | 6 | 4 |
| 3 | 0.7 | 2.1 | 7 | 1 |

빈칸당 1점입니다. 현재 gradient는 0행에서 $(-16,-48)$, 1행에서 $(-8,-24)$, 2행에서 $(-4,-12)$입니다. 학습률 0.025를 곱하여 두 가중치를 함께 갱신합니다.

## 6. Backpropagation — 15점

### (1) 순전파 — 5점

$$
h_{10}=1\times3+2\times1=5,\quad h_{11}=3+1=4
$$

$$
h_{20}=2\times5+4=14,\quad h_{21}=5-2\times4=-3
$$

$$
y=2\times14+(-3)=25
$$

각 값 1점입니다. $h_{21}$이 음수인 것을 유지합니다.

### (2) 출력 미분 — 4점

$\partial h_{10}/\partial a=x_0=3$이며 $h_{11}$은 a에 의존하지 않습니다.

- 경로 $a\to h_{10}\to h_{20}\to y$: $3\times2\times2=12$.
- 경로 $a\to h_{10}\to h_{21}\to y$: $3\times1\times1=3$.

$$
\boxed{\frac{\partial y}{\partial a}=12+3=15}
$$

각 경로의 기여 1점씩, 합계 2점입니다. $h_{21}$의 값이 음수여도 $\partial y/\partial h_{21}=1$입니다. 노드 값의 부호와 local derivative의 부호를 혼동하지 않습니다.

### (3) 손실 미분 — 3점

$$
\frac{\partial\mathcal L}{\partial a}=2(y-24)\frac{\partial y}{\partial a}
=2(25-24)\times15=\boxed{30}
$$

손실 도함수 1점, 연쇄법칙 1점, 값 1점입니다.

### (4) 갱신 — 3점

$$
a^{new}=1-0.01\times30=\boxed{0.7}
$$

손실 gradient 대입 1점, 학습률 반영 1점, 값 1점입니다.

## 7. 다중 분류 손실 — 3점

$$
\boxed{A=-1},\qquad\boxed{B=0.20}
$$

음의 부호에 1점, 정답 클래스 확률에 2점입니다. A에 음의 부호 `-`만 적어 $-\log$를 완성해도 인정합니다.

$$
\mathcal L=-\sum_k t_k\log p_k=\boxed{-\log0.20}
$$

정답은 첫 번째 클래스입니다. 가장 큰 예측 확률 0.70을 넣는 것이 아닙니다.

## 8. 벡터 의미론과 임베딩 — 20점

### (1) 의미 관계 — 4점

| 단어 쌍 | 더 직접적인 관계 | 배점 |
|---|---|---:|
| car / bicycle | Similarity | 1점 |
| car / road | Relatedness | 1점 |

Similarity는 비슷한 의미나 속성의 관계이며, relatedness는 같은 상황·주제에서 연관되는 관계입니다. 자동차와 자전거는 이동수단이라는 공통 속성이 있고, 자동차와 도로는 사용 상황에서 연관되지만 유사한 종류의 사물은 아닙니다. 차이 설명에 2점입니다. Similarity도 넓은 의미의 relatedness에 포함될 수 있습니다.

### (2) Cosine similarity — 4점

$$
\cos(u,v)=\frac{u\cdot v}{\lVert u\rVert_2\lVert v\rVert_2}
$$

$$
u\cdot v=2\times1+1\times2+2\times2=8
$$

$$
\lVert u\rVert_2=\sqrt{4+1+4}=3,\qquad
\lVert v\rVert_2=\sqrt{1+4+4}=3
$$

$$
\boxed{\cos(u,v)=\frac89}
$$

공식, 내적, 두 길이, 최종 값에 각 1점입니다.

### (3) TF-IDF — 4점

$$
tf=1+\log_{10}100=\boxed3,\qquad
idf=\log_{10}\frac{10000}{100}=\boxed2
$$

$$
TF\text{-}IDF=3\times2=\boxed6
$$

각 값 1점입니다. **같은 문서 안에서 여러 번 나와도 df에는 그 문서 한 개분인 1만 기여**합니다. 이에 1점을 부여합니다. Count는 token 수, df는 등장한 문서 수입니다.

### (4) PMI — 4점

$$
PMI(w,c)=\log_2\frac{P(w,c)}{P(w)P(c)}
$$

독립일 때의 동시출현 확률은 $0.2\times0.2=\boxed{0.04}$입니다.

$$
PMI=\log_2\frac{0.02}{0.04}=\log_2\frac12=\boxed{-1}
$$

음의 PMI는 독립일 때 예상되는 것보다 더 드물게 함께 등장한다는 뜻입니다. 여기서는 독립 가정의 절반입니다. 공식, 독립 확률, PMI 값, 부호 해석에 각 1점입니다. PPMI를 묻지 않았으므로 음수를 임의로 0으로 자르지 않습니다.

### (5) Word2vec — 4점

```text
(fresh, enjoy)
(fresh, fruit)
```

중심 단어가 입력이고 좌우 한 칸의 단어가 예측 대상입니다. 각 pair에 1점이며 window 밖의 `we`, `daily`는 포함하지 않습니다.

Negative sampling은 매번 전체 어휘를 비교하는 대신 관측 쌍과 소수의 샘플링된 비교용 쌍을 구분하도록 학습하여 계산 부담을 줄이는 방법입니다. 이 취지의 설명에 1점입니다. 샘플링된 단어가 중심 단어와 절대 함께 등장하지 않는다는 뜻은 아닙니다.

표준 Word2vec은 **static embedding**입니다. 이에 1점입니다. 문맥을 이용해 학습하지만 학습 후 같은 어휘 항목에는 같은 벡터를 사용합니다.
