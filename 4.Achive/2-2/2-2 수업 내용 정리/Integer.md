- Unsigned integer: $\displaystyle\sum_{i=0}^{w-1} {x_i \cdot 2^i}$ 
- Two's complement: $-x_{w-1} \cdot 2^{w-1} + \displaystyle \sum_{i=0}^{w-2} {x_i \cdot 2^i}$ 
- Umin = 00...0, Umax = 11...1 = $2^w -1$ 
- Tmin = 100...0 = $-2^{w-1}$, Tmax = 011...1 = $2^{w-1} -1$

- U와 T를 상호 변환할 때 bit pattern이 보존된다.
- U와 T를 모두 사용한 식을 비교할 때는 T를 모두 U로 변환하여 비교한다. e.g. -1>0U

- T를 extension하는 방법: sign bit을 부족한 bit수 만큼 앞에 복사하여 추가한다. 

- U를 truncating할 때나 overflow가 발생하여 carry를 처리할 때는 바꾸려고 하는 bit에 맞게 modular arithmetic을 하여 처리한다. (drop the overflow bits)

- Multiplication
	- for U: we need up to 2w bits ($0\leq x \times y \leq (2^w -1)^2 = 2^{2w}-2^{w+1}+1$)
	- for T_min: we need up to 2w-1 bits ($x \times y \leq (-2^{w-1}) \times (2^{w-1}-1) = -2^{2w-2}-2^{w-1}$ plus sign bit)
	- for T_max: we need up to 2w bits ($x \times y \leq (-2^{w-1})^2 = 2^{2w-2}$ plus sign bit) but only $(T_{min})^2$ need 2w bits
	- machine에서 U multiplication을 하면 2w가 필요하지만 high w bits를 버리고 w bits로 표현한다. 
	- multiplication 보다는 shift를 잘 사용해서 연산 속도를 높히자