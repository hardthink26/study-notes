# item6: Always Surround Single-Element Tuples with Parentheses

### def 
:tuple을 형성하는 것은 parentheses가 아니라 comma이다.

**example** 
```python
first = (1, 2, 3)
second = 1, 2, 3
third = 1, 2, 3, 
assert first == second == third 
# 전부 tuple로 인식 
```

**caution** 
```python 
first = (1) # int임 
second = (1),# tuple임 
assert first != second 
---
first = func(user),# 의도치 않게 single element tuple이 될 수 있음 
```

### 배울 점 
원소 하나 tuple표현 시 (value,)형식을 이용하자! 

## Item 7: Consider Conditional Expressions for simple inline Logic 
