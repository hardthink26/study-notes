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
- Aggregate functions perform a specific operation over all rows in a group. 
	- **common aggregate functions** 
		- `max()`
		- `min()`
		- `avg()`
		- `sum()`
		- `count()` 

## Implicit versus Explicit Groups 
- Is this possble? 
```sql 
SELECT customer_id,
MAX(amount) max_amt, 
COUNT(*) num_payments 
```
- 이 쿼리는 에러 발생 
	- **don't explicitly how the data should be grouped** 

## Counting Distince Values 
- 중복을 제외한 값을 보고 싶다면? 
	- `distinct` 사용 가능 
```sql 
SELECT COUNT(customer_id) num_rows,
COUNT(DISTINCT customer_id) num_customers
FROM payment 
```

## Using Expressions 
```sql 
SELECT MAX(datediff(return_date, rental_date))
FROM rental
```


## How Nulls are Handled 
- if you add null in your database 
	- row에는 존재로써 추가된 값 
	- 하지만 총합, 평균 등등에는 반영 x 

## Generating Groups 

### Single-Column Grouping 

```sql 
SELECT actor_id, count(*)
FROM film_actor 
GROUP BY actor_id; 
```


### Multicolumn Grouping 
```sql 
SELECT fa.actor_id, f.rating, count(*)
FROM film_actor fa 
INNER JOIN film f
ON fa.film_id = f.film_id
GROUP BY fa.actor_id, f.rating
ORDER BY 1, 2; 
```

### GROUPING VIA EXPRESSIONS 
```sql 
SELECT extract(YEAR FROM rental_date) year,
COUNT(*) how_many 
FROM rental 
GROUP BY extract(YEAR FROM rental_date); 
```

### Generating Rollups 
- **왜 사용? ** 
	- Group by와 함께 쓰여 subtotal, total을 구할 때 씁니다. 
```sql 
SELECT fa.actor_id, f.rating, count(*)
FROM film_actor fa
INNER JOIN film f
ON fa.film_id = f.film_id 
GROUP BY fa.actor_id, f.rating WITH ROLLUP 
ORDER BY 1, 2; 
```


### Group filter conditions 
- use them after the groups have been generated. 
```sql 
SELECT fa.actor_id, f.rating, count(*)
FROM film_actor fa 
INNER JOIN film f
ON fa.film_id = f.film_id
WHERE f.rating IN ('G', 'PG')
GROUP BY fa.actor_id, f.rating 
HAVING count(*) > 9 ; 
```
- 그룹화 전/후로 filter out시 주의 !! 
	- where clause에는 count(*)를 쓸 수 없음 