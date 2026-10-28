
## Outer Joins 
- definition 
	- 기존의 inner join이 모든 테이블의 row를 담을 수 있을까? 
```sql 
SELECT f.film_id, f.title, count(i.inventory_id) num_copies
FROM film f 
LEFT OUTER JOIN inventory i
ON f.film_id = i.film_id
GROUP BY f.film_id, f.title; 
```
- `left outer join`
	- instructs the server to include all rows from the table on the left side of the join
	- include columns from the table on the right side of the join if the join is successful. 
```sql 
mysql> SELECT f.film_id, f.title, i.inventory_id

    -> FROM film f

    -> LEFT OUTER JOIN inventory i

    -> ON f.film_id = i.film_id

    -> WHERE f.film_id BETWEEN 13 AND 15; 
+---------+----------------+--------------+

| film_id | title          | inventory_id |

+---------+----------------+--------------+

|      13 | ALI FOREVER    |           67 |

|      13 | ALI FOREVER    |           68 |

|      13 | ALI FOREVER    |           69 |

|      13 | ALI FOREVER    |           70 |

|      14 | ALICE FANTASIA |         NULL |

|      15 | ALIEN CENTER   |           71 |

|      15 | ALIEN CENTER   |           72 |

|      15 | ALIEN CENTER   |           73 |

|      15 | ALIEN CENTER   |           74 |

|      15 | ALIEN CENTER   |           75 |

|      15 | ALIEN CENTER   |           76 |

+---------+----------------+--------------+
```
- see `null`value 
	- if the join condition fails, any columns retrieved from the outer-joined table will be `null`

### LEFT versus Right outer joins 
- `left outer join`
	- The keyword `left` indicates that the table on the left side of the join is responsible for determining the number of rows in the result set
	- the tablle on the right table side is used to provide column values whenever a match is found. 
```sql 
mysql> SELECT f.film_id, f.title, i.inventory_id

    -> FROM inventory i

    -> RIGHT OUTER JOIN film f

    -> ON f.film_id = i.film_id

    -> WHERE f.film_id BETWEEN 13 AND 15;
+---------+----------------+--------------+

| film_id | title          | inventory_id |

+---------+----------------+--------------+

|      13 | ALI FOREVER    |           67 |

|      13 | ALI FOREVER    |           68 |

|      13 | ALI FOREVER    |           69 |

|      13 | ALI FOREVER    |           70 |

|      14 | ALICE FANTASIA |         NULL |

|      15 | ALIEN CENTER   |           71 |

|      15 | ALIEN CENTER   |           72 |

|      15 | ALIEN CENTER   |           73 |

|      15 | ALIEN CENTER   |           74 |

|      15 | ALIEN CENTER   |           75 |

|      15 | ALIEN CENTER   |           76 |

+---------+----------------+--------------+

11 rows in set (0.00 sec)
```
- 명심할 것 
	- both version of the query are performing outer joins
		- the keyword `left` and `right` are just to tell server which table is allowed to have gaps in the data. 
- **tip**
	- 왠만해선 `right outer join`볼 일 없음, 지원안하는 서버도 있음 
	- 되도록 `left outer join`쓸 것 

### Three-Way Outer joins 
- example: want to outer-join one table with two other tables. 

```SQL 
mysql> SELECT f.film_id, f.title, i.inventory_id, r.rental_date

    -> FROM film f

    -> LEFT OUTER JOIN inventory i

    -> ON f.film_id = i.film_id

    -> LEFT OUTER JOIN rental r

    -> ON i.inventory_id = r.inventory_id

    -> WHERE f.film_id BETWEEN 13 AND 15;

+---------+----------------+--------------+---------------------+

| film_id | title          | inventory_id | rental_date         |

+---------+----------------+--------------+---------------------+

|      13 | ALI FOREVER    |           67 | 2005-07-31 18:11:17 |

|      13 | ALI FOREVER    |           67 | 2005-08-22 21:59:29 |

|      13 | ALI FOREVER    |           68 | 2005-07-28 15:26:20 |

|      13 | ALI FOREVER    |           68 | 2005-08-23 05:02:31 |

|      13 | ALI FOREVER    |           69 | 2005-08-01 23:36:10 |

|      13 | ALI FOREVER    |           69 | 2005-08-22 02:12:44 |

|      13 | ALI FOREVER    |           70 | 2005-07-12 10:51:09 |

|      13 | ALI FOREVER    |           70 | 2005-07-29 01:29:51 |

|      13 | ALI FOREVER    |           70 | 2006-02-14 15:16:03 |

|      14 | ALICE FANTASIA |         NULL | NULL                |

|      15 | ALIEN CENTER   |           71 | 2005-05-28 02:06:37 |

|      15 | ALIEN CENTER   |           71 | 2005-06-17 16:40:03 |

|      15 | ALIEN CENTER   |           71 | 2005-07-11 05:47:08 |

|      15 | ALIEN CENTER   |           71 | 2005-08-02 13:58:55 |

|      15 | ALIEN CENTER   |           71 | 2005-08-23 05:13:09 |

|      15 | ALIEN CENTER   |           72 | 2005-05-27 22:49:27 |

|      15 | ALIEN CENTER   |           72 | 2005-06-19 13:29:28 |

|      15 | ALIEN CENTER   |           72 | 2005-07-07 23:05:53 |

|      15 | ALIEN CENTER   |           72 | 2005-08-01 05:55:13 |

|      15 | ALIEN CENTER   |           72 | 2005-08-20 15:11:48 |

|      15 | ALIEN CENTER   |           73 | 2005-07-06 15:51:58 |

|      15 | ALIEN CENTER   |           73 | 2005-07-30 14:48:24 |

|      15 | ALIEN CENTER   |           73 | 2005-08-20 22:32:11 |

|      15 | ALIEN CENTER   |           74 | 2005-07-27 00:15:18 |

|      15 | ALIEN CENTER   |           74 | 2005-08-23 19:21:22 |

|      15 | ALIEN CENTER   |           75 | 2005-07-09 02:58:41 |

|      15 | ALIEN CENTER   |           75 | 2005-07-29 23:52:01 |

|      15 | ALIEN CENTER   |           75 | 2005-08-18 21:55:01 |

|      15 | ALIEN CENTER   |           76 | 2005-06-15 08:01:29 |

|      15 | ALIEN CENTER   |           76 | 2005-07-07 18:31:50 |

|      15 | ALIEN CENTER   |           76 | 2005-08-01 01:49:36 |

|      15 | ALIEN CENTER   |           76 | 2005-08-17 07:26:47 |

+---------+----------------+--------------+---------------------+

32 rows in set (0.01 sec)
```

## CROSS JOIN
- cartesian product 
	- essentially the result of joining nultiple tables without specifying any join conditions. 

## Natural Joins 
- you can choose a join type that allows you to name the tables to be joined but lets the db server determing what the join conditions need to be
	- This type relies on identical column names across multiple tables to infer the proper join conditions. 

- 따로 제약을 지정하지 않으면 의도치 않은 결합이 가능 
- 피할려면 subquery's column을 제약한 후 사용 
```sql 
mysql> SELECT cust.first_name, cust.last_name, date(r.rental_date)

    -> FROM 

    -> (SELECT customer_id, first_name, last_name

    -> FROM customer

    -> ) cust 

    -> NATURAL JOIN rental r;
```


