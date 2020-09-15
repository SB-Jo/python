함수를 객체 취급하기 때문에 일급함수 개념으로 코딩 가능하다  
파이썬 함수 특징
1. 런타임 초기화 가능
2. 변수 등에 할당 가능
3. 함수 인수로 전달 가능
4. 함수 결과로 반환 가능


```python
# 함수 객체 예제
def factorial(n):
    '''Factorial Function -> n : int'''
    if n == 1: # n < 2
        return 1
    return n * factorial(n-1)

class A:
    pass
```


```python
factorial(5)
```




    120




```python
factorial.__doc__
```




    'Factorial Function -> n : int'




```python
# 둘 다 class 취급한다
print(type(factorial), type(A))
```

    <class 'function'> <class 'type'>
    


```python
# 함수만 갖고 있는 속성
set(sorted(dir(factorial))) - set(sorted(dir(A)))
```




    {'__annotations__',
     '__call__',
     '__closure__',
     '__code__',
     '__defaults__',
     '__get__',
     '__globals__',
     '__kwdefaults__',
     '__name__',
     '__qualname__'}




```python
factorial.__name__
```




    'factorial'




```python
factorial.__code__
```




    <code object factorial at 0x00000203FCBC6030, file "<ipython-input-1-addb817bbc80>", line 2>




```python
# 함수를 변수에 할당
var_func = factorial
```


```python
var_func(5)
```




    120




```python
# map 객체
map(var_func, range(1, 11))
```




    <map at 0x203fcc877c8>




```python
list(map(var_func, range(1, 6)))
```




    [1, 2, 6, 24, 120]



# 고위 함수(Higher-order function)
- 함수에 인수로 전달 및 함수로 결과 반환 가능
- map, reduce, filter 등


```python
# 1, 3, 5만 실행
list(map(var_func, filter(lambda x: x%2, range(1,6))))
```




    [1, 6, 120]




```python
[var_func(i) for i in range(1,6) if i % 2]
```




    [1, 6, 120]



## reduce
- 원래 파이썬에서 기본으로 제공해줬으나 많이 사용되지 않아 제외되었다
- 이전의 결과값을 누적시킨다


```python
from functools import reduce
from operator import add
```


```python
reduce(add, range(1,11))
```




    55




```python
sum(range(1,11))
```




    55



## lambda 함수
- 일반 함수 형태로 리팩토링을 권장한다
 - 이름 없는 함수가 가독성을 조금 떨어트리기 때문


```python
# (((1+2)+3)+4)+5 ...
reduce(lambda x, t: x + t, range(1,11))
```




    55



# Callable
- 호출 연산자 -> 메소드 형태로 호출 가능한지 확인
- \__call__


```python
# 호출 가능한지 확인
# str(), list()
callable(str), callable(list), callable(var_func), callable(3.14), callable(1)
```




    (True, True, True, False, False)




```python
import random

# 로또 추첨 클래스 선언
class LottoGame:
    def __init__(self):
        self._balls = [n for n in range(1, 46)]

    def pick(self):
        random.shuffle(self._balls)
        return sorted([random.choice(self._balls) for n in range(6)])
    # callable하게 만들기
    def __call__(self):
        return self.pick()
```


```python
# 객체 생성
game = LottoGame()
```


```python
# 게임 실행
game.pick()
```




    [2, 11, 19, 20, 31, 31]




```python
# 호출하기
game()
```




    [6, 11, 14, 17, 31, 40]




```python
callable(game)
```




    True



# 다양한 매개변수 입력


```python
def args_test(name, *contents, point=None, **attrs):
    return '<agrs_test> -> ({}) ({}) ({}) ({})'.format(name, contents, point, attrs)
```


```python
args_test('test1')
```




    '<agrs_test> -> (test1) (()) (None) ({})'




```python
args_test('test1', 'test2')
```




    "<agrs_test> -> (test1) (('test2',)) (None) ({})"




```python
args_test('test1', 'test2', 'test3')
```




    "<agrs_test> -> (test1) (('test2', 'test3')) (None) ({})"




```python
 args_test('test1', 'test2', 'test3', id='admin')
```




    "<agrs_test> -> (test1) (('test2', 'test3')) (None) ({'id': 'admin'})"




```python
args_test('test1', 'test2', 'test3', id='admin', point=7)
```




    "<agrs_test> -> (test1) (('test2', 'test3')) (7) ({'id': 'admin'})"




```python
args_test('test1', 'test2', 'test3', id='admin', password='1234', point=7)
```




    "<agrs_test> -> (test1) (('test2', 'test3')) (7) ({'id': 'admin', 'password': '1234'})"



# 함수 Signature
- 함수 인자 정보를 전달해준다


```python
from inspect import signature
```


```python
sg = signature(args_test)
print(sg)
```

    (name, *contents, point=None, **attrs)
    


```python
print(sg.parameters)
```

    OrderedDict([('name', <Parameter "name">), ('contents', <Parameter "*contents">), ('point', <Parameter "point=None">), ('attrs', <Parameter "**attrs">)])
    


```python
for name, param in sg.parameters.items():
    print(name, param.kind, param.default)
```

    name POSITIONAL_OR_KEYWORD <class 'inspect._empty'>
    contents VAR_POSITIONAL <class 'inspect._empty'>
    point KEYWORD_ONLY None
    attrs VAR_KEYWORD <class 'inspect._empty'>
    

# partial
- 인수 고정 후 콜백 함수에 사용
- 하나 이상의 인수가 이미 할당된(채워진) 함수의 새 버전 반환


```python
from operator import mul
from functools import partial
```


```python
print(mul(10, 100))
```

    1000
    


```python
# mul함수의 인자 하나를 5로 고정
five = partial(mul, 5)
```


```python
# 고정추가
six = partial(five, 6)
```


```python
print(type(five))
```

    <class 'functools.partial'>
    


```python
five(100)
```




    500




```python
five(100, 10)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-52-a002550946eb> in <module>
    ----> 1 five(100, 10)
    

    TypeError: mul expected 2 arguments, got 3



```python
six()
```




    30




```python
[five(i) for i in range(1, 11)]
```




    [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]




```python
list(map(five, range(1, 11)))
```




    [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]




```python

```
