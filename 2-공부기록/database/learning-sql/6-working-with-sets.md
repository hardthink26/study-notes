## Set theory in practice 
- **keep in mind** 
	- 적용하려는 data set은 반드시 같은 개수의 column을 가져야 함 
	- 적용하려는 column의 데이터 타입들은 서로 일치하여야 함 

## Set Operators 

### the union Operator 
- **union all** Versus **union** 
	- union all은 데이터 중복으로 표현한다. 

### the except Operator 
- **except** Versus **except all** 
	- except all은 중복 발생된 데이터를 한번 처리하고 except는 중복 발생 데이터를 모두 처리한다. 

### Set Operator Rules 

- **Sorting Compound Query Results** 
	- if order by clause로 정렬한다 가정, 
		- column names로 정렬시, 두 데이터 셋의 aliases를 맞추는게 좋음 
```sql 
SELECT a.first_name fname, a.last_name lname
UNION ALL
SELECT c.first_name, c.last_name 
ORDER BY lname, fname 
```

### Set Operator Precedence 

- **keep in mind** 
	- The ANSI specification calls for the **intersect** operator to have precedence over the other set operators 
	- dictate the order in which queries are combined by enclosing multiple queries in parentheses. 

