
Space-based tokenization
- 매우 쉬운 방법
- 중국어나 일본어 같이 space가 없는 말은?
- want라는 단어를 알아도 wanted는 모른다 -> 인간이 보기에는 비슷한 단어임
Char-level tokenization
- wanted가 왔을 때 w, a, n, t, e, d가 모두 있지만 의미를 파악하기 어렵다
위 두 방법을 모두 각자의 한계가 있다. 우리가 원하는 것은 wanted를 want/ed로 나누는 subword-level 단계로 내려 파악하는 것이다. 

BPE Tokenization
- 글이 있을 때 이를 단어단위(space-base)로 나누고 개수를 센다. 이후에는 2개의 방법이 있다.

1st 방법
1. 모든 단어를 char단위로 나누고, 글에 있던 모든 종류의 char를 vocab에 추가한다. 
2. 크기가 2인 window를 이용하여 split된 단어에서 pair를 찾으며 무슨 pair가 가장 빈도수가 높은지 확인한다. 이후 가장 빈도수가 높은 pair를 vocab에 추가하고 split된 단어목록에서 해당 pair를 merge한다. 
3. 2를 반복한다. 
4. 정해둔 # of iteration or # of vacab를 달성했을 때 반복을 종료한다. 

2nd 방법
1. 모든 단어를 char단위로 나누고, 글에 있던 모든 종류의 char를 vocab에 추가한다. 이때 첫번째가 아닌 char에는 앞에 ##을 붙여서 표시한다. 이후 모든 종류의 char를 vocab에 추가한다.
2. 1st 방법과 같이 pair를 찾고 빈도수 대신 
   $$
score = \frac{{freq\ of\ pair}}{freq\ of\ first\ element\ \times \ freq\ of\ second\ element}
   $$를 계산하여 가장 높은 pair를 vocab에 추가하고 splite된 단어 목록에서 해당 pair를 merge한다. 
3. 2를 반복한다. 
4. 정해둔 # of iteration or # of vacab를 달성했을 때 반복을 종료한다. 
5. 나중에 새로운 단어를 만났을 때 뒤에서부터 짤라가면서 남아있는 앞의 부분이 vocab에 있는지 확인한다. 
6. 5를 통해서 새로운 단어를 파악할 수 있음
