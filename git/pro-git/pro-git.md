## viewing the Commit History 

### commit history전체를 보고 싶다면 ==git log== 명령어 사용! 

### 깃 명령어의 유용한 옵션들 
- ==-p== , ==--patch== : 각 커밋간의 차이를 설명 
- ==-숫자== : 터미널에 나오는 history 제한 가능 


**언제 사용?** 
- code review 
- collaborator들이 커밋한 내용을 점검 

### 각 커밋당 요약된 stat을 보고 싶다면? 
- ==--stat== : 어떤 파일이 얼마나 바뀌었는지 체크 가능 

### 커밋의 아웃풋 포멧을 바꾸고 싶다면? 
- ==--pretty==:

**example**
```zsh
	# 각 커밋당 한 줄씩 출력합니다. 
	% --pretty=oneiline
```


### 각 커밋간의 관계(브렌치, 병합)등을 알고 싶다면? 
```zsh
# 커밋을 그래프 관계로 표현합니다. 특히 브렌치,병합 history 볼 때 사용합니다. 
% git log --graph 
```
