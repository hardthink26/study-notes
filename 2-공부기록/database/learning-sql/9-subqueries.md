- The most powerful features of the SQL language 
	- filter data
	- generate values 
	- construct temporary data sets 

### What is a Subquery? 

- def: A query contained within another SQL statement
	- always enclosed within parentheses
	- return a result set that may consist of: 
		- A single row with a single column 
		- Multiple rows with a single column 
		- Multiple rows having multiple columns 
	- when the containing statement has finished executing, the data returned by any subqueries is discarded, making a subquery act like  a temporary table with **statement scope** 

```sql 
mysql> SELECT customer_id, first_name, last_name

    -> FROM customer 

    -> WHERE customer_id = (SELECT MAX(customer_id) FROM customer);

+-------------+------------+-----------+

| customer_id | first_name | last_name |

+-------------+------------+-----------+

|         599 | AUSTIN     | CINTRON   |

+-------------+------------+-----------+

1 row in set (0.00 sec)
```
- you can take the value the subquery returned and substitute it into the righthand expressions of the filter condition in the containing query 

### Subquery Types 

- **noncorrelated subqueries** 
	- completely self-contained 
- **correlated subqueries** 
	- reference columns from the containing statement 

### Noncorrelated subqueries 

- def: executed alone and doesn't reference anything from the containing statement. 
- **scalar subqeury** 
	- return a result set containing a single row and column 

- **if using a subquery in an equality but the subquery returns more than one-row, will receive an error. 
	- A single thing can't be equated to a set of things 

### Multiple-Row, Single-Column Subqueries 

- if subquery returns more than one row, will not be able to use it on one side of equality condition 

- **The in and not in operators** 
```sql 
SELECT country_id 
FROM country 
WHERE country IN ('Canada', 'Mexico') 
```

- if condition is met, the row is added to the result set 

- not in operator 
```sql 
SELECT city_id, city 
FROM city 
WHERE country_id NOT IN 
(SELECT country_id
FROM country
WHERE country IN ('Cananda', 'Mexico')); 
```

### the all operator 

- the `all` operator allows you to make comparsions between a single value and every value in a set. 
- To build such a condition, need to use one of the comparison operators(=, <>, > ...) in conjunction with the `all` operator. 

```sql 
SELECT first_name, last_name 
FROM customer 
WHERE customer_id <> ALL
(SELECT customer_id
FROM payment 
WHERE amount = 0); 
```

### the any operator 
- Like the `all` operator, the `any` operator allows a value to be compared to the members of a set of values
- unlike `all`, a condition using the `any` operator evaluates to `true` as soon as a single comparison is favorable 

```sql 
mysql> SELECT customer_id, sum(amount)

    -> FROM payment 

    -> GROUP BY customer_id 

    -> HAVING sum(amount) > ANY 

    -> (SELECT sum(p.amount)

    -> FROM payment p

    -> INNER JOIN customer c

    -> ON p.customer_id = c.customer_id

    -> INNER JOIN address a

    -> ON c.address_id = a.address_id

    -> INNER JOIN city ct

    -> ON a.city_id = ct.city_id

    -> INNER JOIN country co

    -> ON ct.country_id = co.country_id

    -> WHERE co.country IN ('Bolivia', 'Paraguay', 'Chile')

    -> GROUP BY co.country

    -> ); 

+-------------+-------------+

| customer_id | sum(amount) |

+-------------+-------------+

|         137 |      194.61 |

|         144 |      195.58 |

|         148 |      216.54 |

|         178 |      194.61 |

|         459 |      186.62 |

|         526 |      221.55 |

+-------------+-------------+

6 rows in set (0.03 sec)
```

### Multicolumn Subqueries 

- you can use subqueries that return two or more columns 
```sql 
mysql> SELECT actor_id, film_id

    -> FROM film_actor 

    -> WHERE (actor_id, film_id) IN

    -> (SELECT a.actor_id, f.film_id

    -> FROM actor a

    -> CROSS JOIN film f

    -> WHERE a.last_name = 'MONROE'

    -> AND f.rating = 'PG');

+----------+---------+

| actor_id | film_id |

+----------+---------+

|      120 |      63 |

|      120 |     144 |

|      120 |     414 |

|      120 |     590 |

|      120 |     715 |

|      120 |     894 |

|      178 |     164 |

|      178 |     194 |

|      178 |     273 |

|      178 |     311 |

|      178 |     983 |

+----------+---------+

11 rows in set (0.00 sec)
```
- with a single subquery that returns two columns instead of two subqueries that each return a single column.
- the subquery in this example uses a type of join called a **cross join** which return all combinations of actors named Monroe and all films rated 'PG'. 

### Correlated Subqueries 
- A **correlated Subquery** is dependent on its containing statement from which it references one or more columns. 
- the correlated subquery is executed once for each candidate row(rows that might be in the final results.
```sql 
mysql> SELECT c.first_name, c.last_name

    -> FROM customer c

    -> WHERE 20 = 

    -> (SELECT count(*) FROM rental r

    -> WHERE r.customer_id = c.customer_id);

+------------+-------------+

| first_name | last_name   |

+------------+-------------+

| LAUREN     | HUDSON      |

| JEANETTE   | GREENE      |

| TARA       | RYAN        |

| WILMA      | RICHARDS    |

| JO         | FOWLER      |

| KAY        | CALDWELL    |

| DANIEL     | CABRAL      |

| ANTHONY    | SCHWAB      |

| TERRY      | GRISSOM     |

| LUIS       | YANEZ       |

| HERBERT    | KRUGER      |

| OSCAR      | AQUINO      |

| RAUL       | FORTIER     |

| NELSON     | CHRISTENSON |

| ALFREDO    | MCADAMS     |

+------------+-------------+

15 rows in set (0.01 sec)
```

### THE EXISTS OPERATOR 
- The most common operator used to build conditions that utilize correlated subqueries is the `exists` operator. 
- **언제 사용?** 
	- when you want to identify that a relationship exists without regard for the quantity.
```sql
mysql> SELECT c.first_name, c.last_name

    -> FROM customer c

    -> WHERE EXISTS

    -> (SELECT 1 FROM rental r

    -> WHERE r.customer_id = c.customer_id

    -> AND date(r.rental_date) < '2005-05-25');

+------------+-------------+

| first_name | last_name   |

+------------+-------------+

| CHARLOTTE  | HUNTER      |

| DELORES    | HANSEN      |

| MINNIE     | ROMERO      |

| CASSANDRA  | WALTERS     |

| ANDREW     | PURDY       |

| MANUEL     | MURRELL     |

| TOMMY      | COLLAZO     |

| NELSON     | CHRISTENSON |

+------------+-------------+

8 rows in set (0.02 sec)
```
- Using the `exists` operator, subquery can return Zero. one, or many rows and the condition simply checks whether the subquery returned one or more rows. 
- since the condition in the containing query only needs to know how many rows have been returned, the actual data the subquery returned is irrelvant. 
- the convention is to specify either `select 1` or `select *` when using `exists` 
- Also, can use `not exists` to check for subqueries that return no rows 

### Data Manipulation Using Correlated Subqueries 
- select문 뿐 아니라 update, delete, insert만에도 다양하게 활용된다!!! 

## When to Use Subqueries 

### Subqueries as Data Sources 
- Since a subquery generates a result set containing rows & columns of data, it's valid to include subqueries in your `from` clause along with tables 

```sql 
SELECT c.first_name, c.last_name,
pymnt.num_rentals, pymnt.tot_payments
FROM customer c
INNER JOIN 
(SELECT customer_id,
count(*) num_rentals, sum(amount) tot_payments
FROM payment 
GROUP BY customer_id
) pymnt 
ON c.customer_id = pymnt.customer_id;
```
- **Subqueries used in the `from` clause must be onocorrelated**
	- because they are executed first 

### Data fabrication 
- 언제 사용? 
	- 나의 데이터베이스에 존재하지 않는 데이터를 만들어야 할 때 사용 가능합니다. 
```sql 
mysql> SELECT pymnt_grps.name, count(*) num_customers

    -> FROM 

    -> (SELECT customer_id,

    -> count(*) num_rentals, sum(amount) tot_payments

    -> FROM payment

    -> GROUP BY customer_id

    -> ) pymnt

    -> INNER JOIN 

    -> (SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit

    -> UNION ALL 

    -> SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit

    -> UNION ALL

    -> SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit

    -> ) pymnt_grps

    -> ON pymnt.tot_payments

    -> BETWEEN pymnt_grps.low_limit AND pymnt_grps.high_limit

    -> GROUP BY pymnt_grps.name;

+---------------+---------------+

| name          | num_customers |

+---------------+---------------+

| Average Joes  |           515 |

| Heavy Hitters |            46 |

| Small Fry     |            38 |

+---------------+---------------+

3 rows in set (0.03 sec)
```
- 그럼  데이터베이스 테이블을 만드는 방법은? 
	- small special purpose tables는 데이터베이스에 낭비일 수 있다. 

### Task-oriented subqueries 

### Common table expressions (CTE)
- A CTE is a named subquery that appears at the top of a query in a `with` clause, which can contain multiple CTEs separated by commas. 