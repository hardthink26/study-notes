
## What Is a Join? 
- ### def 
	- The query instructs the server to use several tables, thereby columns from these tables to be included in the query's result set. 
- ### How? 
	- 보통 foreign key 참조를 통해 참조 
		- **주의**
			- foreign key는 join operator를 위해 있는 것은 아님!!! 

## Cartesian Product 
### def 
- every permutation of several tables
- called cross join 

## Inner Joins 

### def 
- 조건에 매치되지 않은 row는 result set에서 탈락하는 join

### 사용법 
- Join으로 테이블을 묶고 ON으로 각 테이블에 필요 부분만 result set으로 뽑기 

## The ANSI join syntax 
- 현재 사용하는 join syntax는 SQL92 version of the ANSI SQL standard을 따릅니다. 
- 하지만 older version으로 from/where로 구성이 가능합니다. 
	- 장점: 읽고 이해하기 수월합니다. 
	- 단점: 각기 서버마다 문법이 달라 호환성에 문제가 생길 수 있습니다. 

## joining Thress or More Tables 

