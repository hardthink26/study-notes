
## More Fun With **ls** 
### ls 명령어의 정의 
- List directory contents 

**사용법** 
```zsh 
% ls ~/usr 
% ls ~ /usr 
*/이 둘의 결과는 다릅니다. 위의 결과는 홈디렉터리 안 usr폴더를 보는 것이며 아래 ~(tilde)+ /usr의 리스트를 각각 따로 보는 것을 의미합니다.*/ 
```
![명령어 아웃풋 결과](../../../../attachment/commandline_output.png)

### 더 많은 디테일을 원한다면? 
- 명령어 **-l**사용 가능합니다. 
- **사용법** 
```zsh 
% ls -l 
# long format으로 아웃풋 변경 가능합니다. 
```

## Options and Arguments 
- 여러 옵션들을 통해 command의 행동들을 통제 가능합니다. 
- 기본형 
` % command -options arguments` 
-  **example** 
```zsh 
% ls -lt
```

- -l: Display results in long format. 
- -t : Sort by modification time . 


## A Longer Look at Long Format 
- the option *-l* 
	- cause *ls* command to display its results in long format.
- 디렉터리의 접근권한(Directory Permissions)
	- 정의: 디렉터리는 파일 이름과 해당 파일을 가리키는 정보를 관리하는 특수 파일이다. 따라서 일반 파일과 디렉터리에서는 접근 권한의 의미가 다르다. 
	- 타입: 
		- *r(read)* 
			- 일반 파일: 파일 내용 읽기 
			- 디렉터리: 내부 항목의 이름 목록 읽기 
		- *w(write)*
			- 일반 파일: 파일 내용 수정 
			- 디렉터리: 내부 항목 생성, 삭제 ,변경 
		- *x(Execute)*
			- 일반파일: 파일 실행 
			- 디렉터리: 내부 경로 탐색 및 접근 
- 예제로 살펴보기 
```zsh 
drwx------@ 5 soo staff 160 Feb ... 

```
- d: 디렉터리 
- rwx: 소유자에게 읽기·쓰기 ·탐색 권한 부여 
- ---: 소유 그룹에 권한 없음 
- ---: 기타 사용자에게 권한 없음 
- @: macOS 확장 속성 존재 

## Determining a File's Type with file 

- `file` : 파일 종류 확인 
- 사용법 
	- `file filename` 
- linux와의 차이 
	- 기본적 사용법은 동일 
	- 설치된 file버전과 파일 유형 데이터베이스 따라 결과가 달라질 수 있다. 


## Viewing File Contents with less 
`less`: 텍스트 파일 내용 탐색 
- 텍스트 파일의 내용을 페이지 단위로 출력하고, 앞뒤로 이동하거나 특정 문자열 검색 가능 
- 사용법 
	- `less filename` 
- **주요특징** 
	- 파일 수정하지 않고 탐색 
	- 긴 텍스트 파일이나 로그 확인시 유용 
	- 파일 전체를 터미널 화면에 출력하지 않아도 된다. 
	- 파이프라인(|)과 함께 사용 가능 
```zsh 
% ls -l /usr/bin less 
# 출력 결과를 less로 전달해 페이지 단위로 탐색하는 명령어 
```
- linux와의 차이 
	- 설치된 less버전이나 환경 설정에 따라 일부 옵션과 동작이 다를 수 있다. 

- **what is Text?** 
	- Text is a simple one-to-one mapping of characters to numbers. 

## Taking a Guided Tour 
- The file syttem layout on a Linux system is much like that found on other Unix-like systems.
- The design is actually specified in a published standard called the **linux Filesystem Hierarchy Standard** 

### 암기해라 !! 
- `cd`: go into a given directory 
- `ls -l`: List the directory contents
- `file`: if you see an interesting file, determine its contents 
- `less`: if it looks like it might be text, try viewing with this command 

### Linux Filesystem Hierarchy 
-리눅스는 `/`를 최상위로 하는 계층적 파일시스템 구조를 사용한다. 각 디렉터리는 관례적으로 특정 역할을 담당한다. 
- / 
	- home/: 사용자 홈 디렉터리 
	- etc/: 시스템 설정 
	- usr/: 프로그램 및 공유 리소스 
		- bin/: 실행 프로그램 
		- lib/ : 라이브러리 
		- share/: 공유 데이터 
	- var/: 변경되는 데이터 
		- log/: 로그 파일 
	- tmp/: 임시파일 
	- dev/: 장치 파일 
	- proc/: 커널·프로세스 정보(**Linux**)

- 파일 시스템은 단순히 파일을 보관하는 공간이 아니라, 사용자 데이터·프로그램·설정 등을 역할에 따라 구분하는 구조이다. 

## Symbolic Links 
- 