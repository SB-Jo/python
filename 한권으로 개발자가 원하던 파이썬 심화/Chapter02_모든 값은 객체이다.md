# 모든 값은 객체다
- 파이썬에서 변수는 값을 할당할 때 만들어진다.
- 이때 할당되는 모든 값은 '객체(object)'다

## 객체를 직접 만드는 리터럴(literal) 표기법
- 수식이나 글자로 텍스트를 작성하는 방식을 그대로 접목한 것


```python
1
```




    1




```python
'Hello World'
```




    'Hello World'



### 상수(constant)정의


```python
import math
math.pi
```




    3.141592653589793




```python
math.e
```




    2.718281828459045




```python
# 파이썬에서 임의의 상수 정의해 사용할 때, 변수 이름을 모두 대문자로 쓴다
# 변수 이름이 전부 대문자일 경우에는 다른 값 할당하면 안된다
CONST = 100
CONST
```




    100



## 리터럴과 연산자의 묶음인 표현식
- 값을 표시하시는 방식 : 리터럴
- 리터럴 + 연산자(operator) : 표현식
- 논리와 비교 연산자를 넣어 논릿값으로 처리하는 표현식 : 조건식


```python
a = 100
a + 200
```




    300




```python
a.__add__(200)
```




    300




```python
b = '문자'
b.__add__('열')
```




    '문자열'



## 연산자와 스폐셜 메소드 알아보기


```python
# 정수 클래스 int에 있는 __add__ 조회
# slot wrapper -> C언어로 작성된 스폐셜 메소드
int.__add__
```




    <slot wrapper '__add__' of 'int' objects>




```python
(1).__add__(1)
```




    2




```python
int.__add__(1,1)
```




    2




```python
print(dir(int))
```

    ['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
    


```python
(1).__sub__(2)
```




    -1




```python
int.__sub__(1,2)
```




    -1




```python
(2).__mul__(3)
```




    6




```python
int.__mul__(2,3)
```




    6




```python
# 나눗셈은 연산자가 2개
3 / 2
```




    1.5




```python
int.__truediv__(3,2)
```




    1.5




```python
3 // 2
```




    1




```python
int.__floordiv__(3,2)
```




    1




```python

```
