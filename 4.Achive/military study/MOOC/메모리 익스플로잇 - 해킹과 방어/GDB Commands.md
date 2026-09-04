
- (리눅스에서) gdb ./\<excutable file name>
	해당 파일에서 gdb실행

- start
	시작지점 이동
- into ~(ex. register, func, b)
	정보 확인
- near pc 
	현재 pc 주변 정보 확인
- stack
	rsp를 기준으로 스택 정보 확인
- heap	
	힙 정보 확인
- bt	
	현재 instructor pointer를 기준으로 어떤 함수들이 불렸는지 알려주는 back trace 정보 확인
- q 	
	디버거 종료
- disassemble \<func name>
	함수의 어셈블리 정보 확인
- b ~
	브레이크 포인트 설정
	- b *\<func name> +\<n>
		특정 함수에 브레이크 포인트 설정
- d \<n>
	n 번째 브레이크 포인트 삭제
- r \<args>	
	프로그램 실행
- s 
	현재 행 수행 후 함수가 있다면 함수 안으로 들어감
- n
	현재 행 수행 후 함수가 있다면 함수 다음행으로 넘어감
- si, ni 
	s, n과 같으나 instruction단위로 실행
- c	
	다음 브레이크 포인트까지 이동
- finish	
	현재함수를 수행하고 빠져나감
- return 
	현재함수를 수행하지 않고 빠져나감
- wmmap
	현재 메모리 맵핑 정보를 표시함
- search -t qword \<text> 
	문자열이 메모리상에서 어디에 있는지 알려줌, 주로 텍스트에 주소 등을 넣고 검색
- x/8gx \<address>
	address의 주소부터 다음 8개의 메모리 단위를 16진수 형태로 출력
- \<enter>
	이전에 실행했던 gdb 명령어 실행