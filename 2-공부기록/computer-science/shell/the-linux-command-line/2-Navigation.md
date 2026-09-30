
## Understanding the File System Tree 

**기본 암기** 
- pwd: Print name of current working directory 
- cd: Change directory 
- ls : List directory contents 

### hierarchical directory structure 
- def: 윈도우나 리눅스 체제에서 파일을 조직하는 형식을 의미합니다. 폴더라고도 함 
- root directory: 맨 처음 directory를 의미힙니다. 

### window와의 차이점 
- 윈도우: each storage device당 file system tree가 각기 존재합니다. 
- Linux: single file system tree를 가집니다. 

## The Current Working Directory 

### current working directory 
- def: 디렉터리 상 현재 위치한 곳을 의미합니다. 
- 처음 시스템 로그인 시 current working directory는 home directory로 설정입니다. 

## Listing the Contents of a Directory 

### 만약 current working directory의 파일이나 폴더를 보고 싶다면? 
- **is**를 사용합니다. 
- 예제 
```zsh 
% ls 
```


## Absolute Pathnames 

### 작명 원리 
- root directory에서 시작하여 목적지까지 tree branch를 branch단위로 따라갑니다. 
- 예시 
```zsh
% cd /usr/bin 
# usr 안 bin이라는 디렉터리로 이동합니다. 
```


## Relative Pathnames 
### 작명 원리 
- Absolute와 다르게 기준점은 working directory 입니다. 
- special notation: 
	- .(dot) => working directory 의미 
	- .. (dot dot)  => working directory's parent directory 의미 
- convention 
	- ./ 는 보통 생략합니다. 

## About Filenames 

### In mac !! 
- linux와 달리 기본 macOS 파일 시스템은 일반적으로 cas-insensitive이므로 파일명의 대소문자 처리에 차이가 있다. 