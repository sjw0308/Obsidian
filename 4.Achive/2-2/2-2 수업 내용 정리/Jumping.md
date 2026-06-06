- jX instructions
	- jX dst: X에 해당하는 condition이 1이면  dst(instruction address)로 이동한다. 
		- jmp: 1
		- je: ZF
		- js: SF
		- 등등..
	- jX를 이용하여 c수준의 goto문과 1대 1로 대응되는 assembly어를 만들 수 있고, 이를 이용하여 if-else, while, for문을 모두 구현할 수 있다. 

- Jump table structure
	- Jump table structure: switch-case문을 사용할 때 활용할 수 있다. case의 값 차이가 적을 때 사용할 수 있다. 
	- jmp \*{ a }(,{ b }, { c })
		- { a }: 해당 부분에는 jump table의 시작 주소가 들어간다. 
		- { b }: switch에 들어가는 입력 값(index)을 가진 reg가 들어간다. 
		- { c }: size가 들어간다. e.g. int이면 4, long이면 8
	- 구조: case n에 대응되는 assembly code의 instruction address를 x라고 하면, jump table의 시작 주소 + n * { c }인 memory에 x를 넣어둔다. -> { b }에 n이 들어오면 jump instruction에 의해서 x로 이동(memory에 있는 값 앞에 * 를 붙여서 주소로 작용한다. )

