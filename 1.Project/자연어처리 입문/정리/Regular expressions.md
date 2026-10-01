
Regular expression은 NLP이전 text들을 pre-processing하는데 많이 사용한다. 

Disjunction (OR)
- \[~]를 사용하면 ~에 있는 내용들이 or이됨
- e.g. \[1234567890] -> Any one digit
- Range로 표한할 수도 있다. 
- e.g. \[A-Z] -> An uppercase letter
- | 로도 Disjunction을 사용할 수 있다. 
- e.g. yours|mine -> "yours" or "mine"

Not
- ^를 이용해서 not의 의미를 넣을 수 있다. 
- e.g.
	- \[^A-Z] -> Not an uppercase letter
	- \[^Ss] -> Neither S nor s
	- \[e^] -> e or ^

Convenient aliases
- ![[Pasted image 20260922164840.png]]

Wildcards, optionality, repetition
- . -> any char
- me? -> e가 있어도 되고 없어도 됨 (m, me)
- to* -> o가 0개이상 (t, to, too, tooo, ...)
- to+ -> o가 1개이상 (to, too, tooo, ...)

Anchors
- ^ -> sequence의 시작
- $ -> sequence의 마지막
- e.g.
	- ^\[A-Z] -> 맨 처음 나오는 대문자
	- \\.$ -> 맨 마지막 나오는 온점 (그냥 온점은 모든 문자를 의미하므로, \\.을 써서 온점을 의미하게 만듦)

False positives and False negatives
- False negative: 우리가 찾으려던 것을 못찾는 경우 (e.g. the는 The를 찾을 수 없다)
- False Positive: 우리가 찾지 않으려던 것까지 찾는 경우 (e.g. \[tT]he는 there, then등까지 찾아진다)

Substitution
- s/A/B -> A를 B로 바꿔라
- Capture Groups
	- s/(~)/**
	- 이라고 하면, ** 에서 \\1을 쓰면 첫번째에서 찾은 ~를 그대로 사용할 수 있다. 
	- e.g.
		- s/(\[0-9]+)/<\\1> 이면
		- the 35 boxes -> the <35> boxes  로 바뀐다.
	- 만약 capture하고 싶지 않다면?
		- ()안에 내용을 쓰기 전 ?: 를 사용하면 register에 저장하지 않을 수 있다. 

Lookahead Assertions
- (?=pattern) -> pattern이 들어가는 단어
- (?!pattern) -> pattern이 안들어가는 단어
- e.g.
	- /^(?!Volcano) \[A-za-z]+/ -> Volcano로 시작하지 않는 모든 단어(sequence)
