---
sticker: ""
---
[[컴퓨터SW시스템개론]]

- [x]  #task ⏫ 📅 2023-09-25 ✅ 2023-09-28

- problem 1: ~x+1로 -x를 표현할 수 있다. 
- problem 2: 우선 x, y의 부호를 sing_x/y에 각각 저장. sign변수에 두 부호가 다른지 같은지 저장. diff에 x-y를 저장. diff_sign에 x-y의 부호를 저장. min_check변수를 통해 x, y가 t_min일 때 발생할 수 있는 예외를 처리함. 부호가 다르면 x의 부호가 -일 때 1이 나옴 -> (sign & sign_x). 부호가 같으면 diff의 부호가 -일 때 1이 나옴 -> (!sign & diff_sign). x가 t_min이고 가 t_min이 아닐 때는 1, 모두 t_min일 때는 0이 나오도록 설계 -> !((x^t_min) | !(y^t_min)). 이들을 |로 연결하여 값을 return함
- problem 3: uf가 NaN일 때 argument를 return 할 수 있도록 예외 처리하고, 나머지는 0x7fffffff과 &연산을 하여 sign bit을 0으로 만들어줌
- problem 4: 실수는 sign, exp, frac부분으로 나뉜다. 우선 예외처리를 하기 위하여 exp=11...1인 경우 argument를 return한다. exp=00...0인 경우 denormalized되어 있기 때문에 frac을 1만큼 left shift해준다. 이외의 경우 exp를 +1해준다.
- problem 5: 우선 예외처리를 하기 위하여 input integer x가 0이라면 0을 return한다. x=t_min이라면 그에 해당하는 float인 0xcf000000를 return한다. 실수는 sign, exp, frac부분으로 나뉜다. 일단 x의 sign을 sign변수에 저장하고 이를 이용하여 x를 unsigned로 만들어서 ux에 저장한다. 이후 ux에서 MSB(sign bit)이후 가장 처음 1이 나오는 자리를 for문을 이용하여 i에 저장한다. exp는 i에 bias인 0x7f를 더한 값을 저장한다. frac은 i의 크기에 따라 다르다. i가 23보다 작거나 같은 경우 x를 float로 round없이 표현이 가능하다. 우선 ux의 가장 앞의 1을 없애고 (24-i)만큼 left shift시킨다. 