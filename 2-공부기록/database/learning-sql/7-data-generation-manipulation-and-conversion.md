
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
```sql 
SELECT (37* 59) / (83 - (23 * 3));
```

### performing Arithmetic functions 
- ex) 
	- acos(x)
	- atan(x) 
	- cos(x) 
	- in(x) 
	- sin(x) 
	- sqrt(x) 
	- mod(x)
	- pow(x) 
### Controlling Number Precision 

- 올림/ 내림 함수 (ceil, floor)
```sql 
#올림 함수 
SELECT CEIL(82.334);
# -> 83 
```
```sql 
SELECT FLOOR(82.334);
# -> 82 
```

- 반올림 함수 (round)
	- midpoint기준으로 반올림을 합니다. 
	- seconde argu로 반올림 위치를 정할 수 있습니다. 
```sql 
SELECT ROUND(82.0909, 1);
# -> 82.1
```

- 자르기 (truncate)
	- 원하는 위치에서 자르기 가능합니다. 

- round & truncate function은 두 번째 인자로 negative value가 가능합니다. 
```sql 
SELECT ROUND(17, -1); 
# -> 20 
SELECT TRUNCATE(17, -1);
# -> 10 
```


### handling Signed data 
- sign() 
	- return value 
		- -1: 값이 음수 
		- 0: 값이 제로 
		- 1 : 값이 양수 
- abs() 
	- return value 
		- 절댓값을 반환합니다. 

## Working with Temporal Data 
- temporal data is the most involved when it comes to data generation and manipulation. 

### Dealing with Time Zones 
- The world has been sliced into 24 imaginary sections, called **time zone**.
	- **유래** 
		- fifteenth-century navigators set their clocks to the time of day in Greenwich, England called GMT(Greenwich Mean Time)
- 현재는 variation of GMT called Coordinated Universal Time(UTC) 사용합니다. 
	- atomic clock을 베이스로 합니다. 

- (in Mysql) 두 개의 다른 time zone setting이 있습니다. 
```sql 
SELECT @@global.time_zone, @@session.time_zone 
```
- 만약 취리히에 있고, New york에 네트워크를 둔 session을 열 때 
	- `SELECT SET time_zone = 'Europe/Zurich`로  설정 변경 가능합니다. 

### Generating Temporal Data 

- **string representations of temporal data** 
	- server가 원하는 형식의 format을 갖추어야합나다!!
```sql 
# 만약 return_data => Datetime의 형식이라면? 
UPDATE rental 
SET return_date = '2019-09-17 15:30:00'
WHERE rental_id = 33; 
```

### String-to-date conversions 
- if the server is not expecting a **datetime** value or if represent the datetime using a nondefault format, 
	- `cast()`function으로 string에서 date format으로 변환 가능 
	- **예시** 
		- `SELECT CAST('2019-09-17 12:22:00';`
- (IN MYSQL)
	- seperator는 엄격히 요구되지않습니다. 
		- 예시 
			- `2019/09/17 15:30:00'
			- `2019, 09, 17 15, 30, 00`

### Functions for generating dates 

- (in Mysql) 
	- cast의 형식에 맞지않는 format이 들어올 경우 `str_to_date()`를 사용합니다. 
```sql 
UPDATE rental 
SET return_date = STR_TO_DATE('September 17, 2017', '%M %d, %Y')
WHERE rental_id = 22; 
# second argument는 date string의 포멧을 정의합니다. 
```

- str_to_date함수는 datetime, date, or time value를 반환합니다. 

- **만약 현재 시간을 만든다면? ** 
	- bulit-in function을 이용합시다. 
		- CURRENT_TIME, CURRENT_DATE, CURRENT_TIMESTAMP 이용 가능 

### Temporal functions that return dates 
- 하나의 date를 받아서 다른 date 생성하는 함수 
	- date_add function 
```sql 
SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 5 DAY);
```
- LET'S SEE SECOND ARGUMENT 
	- Three elements 
		- interval keyword 
		- the desired quantity 
		- type of interval 
			- second 
			- minute 
			- hour 
			- minute-second -> Number of minutes and seconds, separated by":" 
			- hour-second 
			- year-month 
```sql 
UPDATE rental 
SET return_date = DATE_ADD(return_date, INTERVAL '2:27:11' HOUR_SECOND);
```
- IF you know where you want to arrive  but not how many days is takes to get there.
	- use `last_day()`
- ```sql 
  SELECT LAST_DAY('2017-03-23');
  ```
- 이 함수는 언제나 date값을 반환합니다. 

### temporal functions that return strings 
- `dayname()`
	- to extract a portion of a date or time 
```sql 
SELECT DAYNAME('2019-02-23'); 
```

- `extract()`
	- for extracting from date values 
	- ues the same interval types as the `date_add()` 
```sql 
SELECT EXTRACT(YEAR FROM '2017-09-18 22:19:04');
```

### Temporal functions that return numbers 
- 만약 두 date value 의 차이를 알고 싶다면? 
	- `datediff()`사용합니다. 
```sql 
SELECT DATEDIFF('2019-09-03', '2019-06-21'); 
```
- 앞의 값에서 뒤의 값을 뺍니다. 
- time값은 사용되지않습니다. 

### conversion Functions 
- `cast()` 
```sql 
SELECT CAST('123' AS SIGNED INTEGER); 
```
- provide a  value or expression 
	- **as** keyword 
	- the type to which you want the value converted 

- when converting a string to number 
	- 왼쪽에서 오른쪽으로 읽습니다. 
	- 만약 수치가 아닌 데이터가 있다면? 
		- 변환은 멈추고 warning 을 보냅니다. 
```sql 
SELECT CAST('999ABC333' AS UNSIGNED INTEGER);

+---------------------------------------+

| CAST('999ABC333' AS UNSIGNED INTEGER) |

+---------------------------------------+

|                                   999 |

+---------------------------------------+

1 row in set, 1 warning (0.00 sec)
```