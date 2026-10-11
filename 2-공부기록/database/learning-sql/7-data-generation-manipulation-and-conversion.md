
## Working with String Data 
- 암기사항 
	- CHAR 
		- fixed-length, blank-padded strings를 가집니다. 
	- varchar 
		- variable-length strings를 가집니다. 
	- text 
		- very large variable-length strings를 가집니다. 

## String Generation 
- String field 생성 
```sql 
INSERT INTO string_tbl (char_fld, vchar_fld, text_fld)
VALUES ('A', 
'B',
'C');
```

- 값 수정 query 
```sql
UPDATE string_tbl
SET vchar_fld = 'This is a piece of extremely long varchar data'; 
```

- 현재 mysql version에서는 strict mode 입니다. 
	- 정해진 길이 이상을 넘을 시, exception이 뜹니다. 
	- 과거 버전은 정해진 이상 길이는 잘리고 경고 문구가 뜹니다. 
	- 확인 방법: `SELECT @@session.sql_mode;`로 확인합니다. 

## Including single quotes 
```sql 
UPDATE string_tbl
SET text_fld = 'This string doesn't work'; 
```
- 위와 같은 예시는 작동하지 않습니다. 
	- 서버에서 doesn't의 apostrophe를 end of the string으로 인식합니다. 
	- **해결책** 
```sql 
UPDATE string_tbl
SET text_fld = 'This string didn''t work, but it does now': 
#escape할 부분에 apostrophe를 한 번 더 작성합니다. 
```

## String manipulation 
- Datebase server 는 빌트인 함수를 가지고 있습니다. 
	- String functions that return numbers 
		- Length함수 
			- `SELECT LENGTH(char_fld)`로 씁니다. 
			- 함수의 목적은 길이 측정입니다. 
		- locate, position 함수 
			- 차이는 locate는 어디서부터 serach할지 체크하는 third parameter를 더할 수 있습니다. 
		- strcmp 함수 
			- string comparison function입니다. 
			- `strcmp(first_string, second_string) -> integer'
				- -1: first_string이 second_string보다 먼저 옵니다. 
				- 0: 동일 값입니다. 
				- 1: first_string이 second_string보다 뒤에 옵니다. 

## String function that return strings 
1. concat함수 (in **mysql**)
	- appending additional characters to a stored string
```sql 
UPDATE string_tbl
SET text_fld = CONCAT(text_fld, ', asdf'); 
```

2. insert 함수 
	- 특정 부분에 character를 넣을 때 사용합니다. 
	- 4개의 인자를 갖습니다. 
		- the original string 
		- the position at which to start 
		- the number of characters to replace 
		- the replacement string 
	- **예시** 
		- `SELECT INSERT('goodbye world, 9, 0, cruel ') string;`
3. substring 함수 
	- 특정 부분의 character를 extract할때 사용합니다. 
	- **예시** 
		- `SELECT SUBSTRING('goodbye cruel world', 9, 5);`

## Working with Numeric Data 
