
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