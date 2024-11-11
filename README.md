# SDEV_RANDSOMEWARE 기본 프로그램 README.md

'''
**버전에 따라 실행부를 다르게 설정해야 함을 알게되어, 컴퓨터 버전을 알아보는 과정이 필요했다.\n
이 레포는, 해당 기능을 추가하기 위해 작성되었다.\n
전체 랜섬웨어가 보고 싶다면, 랜섬웨어 팀레포에 가면 확인 가능하다.**

'''
이 프로그램(기본 프로그램)은 설정부-실행부-저장부로 구성되어 있으며

설정부는 
```
1. 윈도우 설정부, 
2. 암호키 설정부,
3. 호스트~게스트(이때 게스트는 윈도우 가상머신으로 한다) 소켓부
```
로 구성되며,


실행부는
```
4. 파일 유형 검사부, 
5. github "tiny AES" 라이브러리를 이용한 암호화부,
6. 4-5를 반복할 수 있도록 file들을 순열반복검사하는 반복문부
```
로 구성된다. 


저장부는 실행부와 함께 혹은 선행되어 작성되어야 하며,
```
7-1. 과정 4에서 타겟으로 설정된 확장자의 파일을 복사하여 .sdev 확장자로 변경하는 .sdev 파일 제작부,
7-2. 과정 5가 완료된 과정 7-1의 원본 파일을 삭제하는 삭제부
```
로 구성된다.
저장부는 실행부의 6번 과정에 삽입되어 반복문의 개수를 최대한 줄이도록 한다.


***
# SDEV_RANDSOMWARE 각 부의 역할 

아래는 각 부의 역할이다. 
편의를 위해 공격자를 A(Attacker), 혹은 서버라고 하고 , 피해자를 V(Victim), 혹은 클라이언트라고 한다.


# <설정부> 
**[1. 윈도우 설정부의 역할]**
윈도우 설정부의 역할은 다음과 같다.
```
1) 클라이언트의 windows 버전 확인
	-windows 11에서 경로명에 Onedrive가 삽입되는 문제 혹은 윈도우 버전으로 인한 추가 이슈를 방지하기 위함.
2) 클라이언트의 사용자명 확인
	-파일 디렉토리를 설정하기 위함.
		
3) 파일 디렉토리 설정
	i) Documents
	[ii] Desktop 
	[iii] Downloads 
	[iv] Users\{User}\Local\AppData\Temp
	로 파일 검사 디렉토리로 설정(구조체로 묶을 것)
```
 
**[2. 암호키 설정부의 역할]**
암호키 설정부의 역할은 다음과 같다. 
```	
1) Github 라이브러리를 사용하여 srand()로 8bit 암호화키 생성
```
 
**[3. 소켓부의 역할]**
소켓부의 역할은 다음과 같다.
```
1) 클라이언트
	프로그램 실행 시, 
	i) 자신의 ip 주소를 서버에 전송
	ii) 자신의 포트 번호를 서버에 전송

2) 서버 
	3.-1)에서 확인한 정보로 클라이언트에
	i) 2.에서 생성된 암호화키를 blocking 방식으로 전송
	ii) 전송한 암호화키를 HKEY_CURRENT_USER\Environment\KEY에 삽입 
```
 
# <실행부>
**[4. 파일 유형 검사부의 역할]**
```
 1) 파일의 hex를 읽어 헤더/푸터를 비교하여 다른 파일과 txt, png, jpg, doc/docx , (mp3, mp4, png, jpg, gif, txt, docx, pptx, xlsx, pdf) 파일을 구분
```
   
**[5. 암호화부의 역할]**
```
 	1) 해당하는 파일을 AES로 암호화한다.
```

**[6. 반복문부의 역할]**
```
 	1) 전체 디렉토리를 검사하는 동안 검사부, 암호화부, 저장부를 반복
```
 
# <저장부>
**[7-1. .sdev 파일 생성부의 역할]**
```
 	1) 4번 과정에 해당하는 파일을 같은 디렉토리에 복사하여 저장함. 이후 5번 과정을 거치면 복사한 파일 확장자를 .sdev로 바꾸어 저장함
```
**[7-2. 원본 파일 삭제부의 역할]**
```
 	1) 7-1번 과정을 거친 파일의 원본 파일을 삭제함
```
