
- P = process(\["excutable file or command", "arg"s])  또는   process("excutable file or command")
	args를 인자로 넘기면서 실행 파일 또는 명령어를 실행하고 실행 프로세스에 대한 핸들러를 P에 저장
	- P.interacive()
		- 실행 중 인 프로세스 P와 상호작용, 터미널을 통해서 데이터 입출력을 가능하게 함

- 데이터 수신에 사용하는 함수들
	- data = P.recv(1024)
		P의 출력 데이터를 최대 1024 바이트까지 받아서 data에 저장
	- data = P.recvline()
		P의 출력 데이터를 개행문자까지 받아서 data에 저장
	- data = P.recvuntil(b"string")
		P의 출력 데이터 중, b"string"가 출력 될 때까지 받아서 저장

- 데이터 송신에 사용하는 함수들
	- P.send(b'A')
		P에 A를 입력
	- P.sendline(b'A')
		P에 A와 개행문자를 입력
	- P.sendafter(b'string', b'A')
		P가 string을 출력하면, A를 입력
	- P.sendlineafter(b'string', b'A')
		P가 string을 출력하면, A와 개행문자를 입력
	우리가 다루는 프로그램이 gets()와 scanf()와 같이 개행문자를 입력의 끝으로 인식하는 함수라면  sendline(), sendlineafter와 같은 함수를 사용해야 한다. 

- 패킹 언패킹
	데이터를 특정 형식으로 변환하고 읽기 위한 필수 작업 (ex. 리틀 엔디언, 빅 엔디언)
	- 패킹
		시스템(CPU)이 사용하는 바이트 배열로 변환
		- 시스템에 전달하게 될 주소/메모리 값에 적용
		- p32(): 32비트 정수 값을 리틀 엔디안 바이트 배열로 변환
		- p64(): 64비트 정수 값을 리틀 엔디안 바이트 배열로 변환
	- 언패킹
		사람이 사용하는 바읕 배열로 변환
		- 시스템으로 얻어온 주소/메모리값에 적용
		- u32(): 32비트 리틀 엔디안 바이트 배열을 정수로 변환
		- u64(): 64비트 리틀 엔디안 바이트 배열을 정수로 변환
