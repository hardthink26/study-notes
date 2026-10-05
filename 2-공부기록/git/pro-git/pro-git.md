## viewing the Commit History 

### commit history전체를 보고 싶다면 **git log** 명령어 사용! 

### 깃 명령어의 유용한 옵션들 
- **-p**, **--patch** : 각 커밋간의 차이를 설명 
- **-숫자** : 터미널에 나오는 history 제한 가능 


>언제 사용?
- code review 
- collaborator들이 커밋한 내용을 점검 

### 각 커밋당 요약된 stat을 보고 싶다면? 
- **--stat** : 어떤 파일이 얼마나 바뀌었는지 체크 가능 

### 커밋의 아웃풋 포멧을 바꾸고 싶다면? 
- **--pretty**:

>example
```zsh
	# 각 커밋당 한 줄씩 출력합니다. 
	% --pretty=oneiline
```


### 각 커밋간의 관계(브렌치, 병합)등을 알고 싶다면? 
```zsh
# 커밋을 그래프 관계로 표현합니다. 특히 브렌치,병합 history 볼 때 사용합니다. 
% git log --graph 
```


## Limiting Log Output 
### git log의 다양한 limiting option 
- 특정 기간 생성한 커밋을 보고 싶다면? 
```zsh 
#2주 전부터 커밋한 자료들만 본다. 
% git log --since=2.weeks
```
- 특정 문자열이 변화를 담은 커밋을 보고 싶다면? 
```zsh 
# verify_auth 함수를 담은 커밋 시점을 본다. 
% git log -S verfiy_auth 
```

- 특정 파일의 변경 사항을 담은 커밋을 보고 싶다면? 
```zsh 
# app/model.py의 변경사항을 담은 커밋을 본다. 
% git log -- app/model.py
```

- **--no-merges**옵션의 사용 
	- 보통 git log는 merge commit도 같이 나온다. ->이것은 정보가 없을 수 있고 로그를 복잡히 한다. 
	- 따라서 나머지 커밋만 볼 때 사용 가능하다. 
```zsh 
% git log --no-merges
```

## Undoing Things 
### 언제 사용? 
- 추가 파일을 빼먹고 커밋 x 
- 커밋 메세지 재작성 필요 시 

### 사용 명령어 
```zsh
% git commit --amend
```

### 예시 
```zsh 
# 모르고 파일 빼먹고 안 올림 
% git commit -m "Initial commit"
% add forgotten_file 
% git commit --amend 
```

***caution*** 
- 이전 커밋 사항을 amend하면 repository history에서 아예 사라짐 
- 주로 사소한 변경점을 마지막 커밋에 더할 때 사용 
- collaborators들과 작업 시엔 특히 주의 

## Unstaging a Staged File 
