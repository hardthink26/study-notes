
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

## joining Three or More Tables 

- ### 언제 사용? 
	- 세 개 이상 테이블을 참조해야 할때 사용 
- ### 방법은? 
	- from clause안에 참조하는 테이블 갯수만큼 inner join, on을 삽입 

## Using Subqueries as Tables 
- **왜 사용할까? 테이블을 각기 다 참조는? ** 
	- performance and readability를 위해 사용됨 

## Using the Same Table Twice 

- **언제 사용?** 
	- 예시) 배우 A & B가 동시에 출연한 영화를 뽑아야할 때 
- 사용법 
```sql 
SELECT f.title 
FROM film f 
INNER JOIN film_actor fa1
ON f.film_id = fa1.film_id
INNER JOIN actor a1
ON fa1.actor_id = a1.actor_id 
# 각 참조 테이블을 각기 다른 네이밍으로 설정 
```

## Self-Joins 

- ### def 
	- join a table to itself 
- ### 왜 사용할까? 
	- 예시) movie table이 있고 어떤 영화는 sequel을 가지고 있음 그렇다면 영화가 영화를 참조하는 구조가 되어 self reference구조를 가지게 됨 