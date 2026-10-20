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