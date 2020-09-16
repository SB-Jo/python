# 함수와 클래스의 문서화


```python
# __doc__ 간단한 설명만 달려 있다
import math
a = math.__doc__
print(a)
```

    This module provides access to the mathematical functions
    defined by the C standard.
    


```python
%%writefile add.py
'''이 모듈에는 덧셈을 계산하는 함수를 제공합니다'''

def add(x,y):
    return x+y
```

    Writing add.py
    


```python
# 작성된 모듈 import 하기
import add
b = add.__doc__
print(b)
```

    이 모듈에는 덧셈을 계산하는 함수를 제공합니다
    


```python
def func(x,y):
    '''첫번째 매개변수 체크'''
    if not isinstance(x, int): # 매개변수 x 값이 정수인지 체크
        raise TypeError('x 정수가 아님')
    
    '''두번째 매개변수 체크'''
    if not isinstance(y, int):
        raise TypeError('y 정수가 아님')
    
    return x+y
```


```python
# 첫 번째 문자열만 문서화. 문서화는 항상 첫 번째에 작성해야 한다
func.__doc__
```




    '첫번째 매개변수 체크'



# 문장을 설명하는 주석
- 모듈을 실행해도 주석에 대한 처리는 아무것도 없다.
- 주석은 문장이나 표현식이 아니므로 추가적 실행이 없다
- 주석은 문서화 처리가 되지 않는다


```python
%%writefile math_.py
'''이 모듈은 기존 수학 모듈을 이용해서 추가로 구성된 모듈입니다'''

# math모듈을 import합니다
import math

# 모듈 내의 변수에 기존 모듈을 할당
math_ = math
```

    Writing math_.py
    


```python
import math_
```


```python
b = math_.math_.fsum.__doc__
print(b)
```

    Return an accurate floating point sum of values in the iterable seq.
    
    Assumes IEEE-754 floating point arithmetic.
    


```python
math_.math_.fsum([1,2,3,4])
```




    10.0



# 변수에 타입 힌트 사용하기


```python
# 타입 힌트만 지정하면 실제 변수가 정의되진 않는다.
x : int
try:
    x
except Exception as e:
    print(e)
```

    name 'x' is not defined
    


```python
y: int
y = 100
try:
    print(y)
except Exception as e:
    print(e)
```

    100
    


```python
def add(x,y):
    z : int
    z = x + y
    return z
add(10, 10)
```




    20




```python
# 왜 None? => 매개변수에 타입힌트 지정하지 않아서
# 함수 내부 타입 힌트는 외부에서 볼 수 없다
add.__annotations__
```




    {}




```python
def add(x:int, y:int):
    return z
add.__annotations__
```




    {'x': int, 'y': int}




```python

```
