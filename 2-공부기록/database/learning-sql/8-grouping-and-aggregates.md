- **in this chapter our goal** 
	- how data can be grouped and aggregated to allow users to interact with it at some higher level of granularity. 

## Grouping Concepts 
- **예시** 
	- 누가 가장 많이 movie film을 렌트했는지 조사하자 
		- `GROUP BY` & `count function`을 활용 ! 
```sql 
SELECT customer_id, count(*)
FROM rental 
GROUP BY customer_id; 
# 각 소비자의 id + 빌린 횟수를 볼 수 있음 
```
- 만약 가장 많이 빌린 순으로 본다면? 
```sql 
SELECT customer_id, count(*)
FROM rental 
GROUP BY customer_id 
ORDER BY 2 DESC; 
```

- 그룹화 한 것을 filter를 해보자 
```sql 
SELECT customer_id, count(*)
FROM rental 
WHERE count(*) >= 40 
GROUP BY customer_id 
```

- 왜 위의 코드는 작동하지 않을까? 
	- `GROUP BY`clause는 `WHERE`clause이후에 동작합니다.
	- can't refer to the aggregate function(count) in where clause. 

- 해결책 
	- Group by의 filtering으로 `having`clause를 씁니다. 
```sql 
SELECT customer_id, count(*)
FROM rental 
GROUP BY customer_id 
HAVING count(*) >= 40; 
```

## Aggregate Functions 
