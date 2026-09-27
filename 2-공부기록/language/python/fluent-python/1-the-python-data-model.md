**Collection API**
- collection = Iterable+Sized+Container로 구성, 아래로는 Sequence, Mapping, Set으로 전문화되며 각 인터페이스의 동작은 special method를 기반으로 한다. 

 **Overview of Special Methods**

## why len is Not a Method 
- Cpython의 bulit-in 객체에서 길이를 더 효율적으로 얻을 수 있도록 하기 위해서이다. 

 