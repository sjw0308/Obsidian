
- ls -a // ll
	현재 디렉토리의 파일들 상세

- cd~
	디렉토리 이동

- shutdown now
	컴퓨터 종료

- rm \<file name>
	해당 파일을 삭제, -r 옵션으로 디렉토리 삭제 가능

- touch \<file name>
	파일 생성

- gcc -o \<ex file name> \<c file name>
	c파일 컴파일 후 실행파일로 만듦

- mv \<file name> \<file name>
	파일을 옮기거나 이름을 바꿀 때 사용

- sudo ~	
	관리자 권한으로 실행
	- sudo apt-get install ~
		다운받을 때 주로 실행

- cp \<original file name> \<new file name>
	original file을 new file로 복사 할 때 사용

- top  //  htop
	현재 실행 중인 프로세스들을 실시간으로 확인

- ps (-ef)
	현재 프로세스를 (모두) 확인
	- ps -ef | grep \<process name>  //  pgrep
		\<process name>을 가진 프로세스를 검색하여 표기

- less, more
	둘 모두 긴 텍스트 내용을 화면단위로 보여주는 도구
	- more은 앞으로만 이동이 가능하지만 less는 양방향 스크롤 및 검색 기능까지 사용이 가능하다
	- more \<file name>
		- \<space bar>  
			다음 페이지 이동
		- \<enter>
			다음 줄로 이동
		- q
			종료
	- less \<file name>
		- \<space bar> // f
			다음 페이지 이동
		- \<enter>
			다음 줄로 이동
		- b
			이전 페이지 이동
		- \<up/down arrow>
			위 아래 스크롤
		- /\<keyword>
			키워드 검색
			- 검색결과에서 n은 다음 검색 결과로, N은 이전 검색 결과로 이동한다.
		- q
			종료