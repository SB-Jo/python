# 파이썬 변수 범위


```python
def func_v1(a):
    print(a)
    print(b)

func_v1(5)
```

    5
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-1-bbe837561d19> in <module>
          3     print(b)
          4 
    ----> 5 func_v1(5)
    

    <ipython-input-1-bbe837561d19> in func_v1(a)
          1 def func_v1(a):
          2     print(a)
    ----> 3     print(b)
          4 
          5 func_v1(5)
    

    NameError: name 'b' is not defined



```python
# 전역 변수
b = 10

def func_v2(a):
    print(a)
    print(b)

func_v2(5)
```

    5
    10
    


```python
# 지역 변수 범위에 b가 있기에 전역변수를 확인하지 않는다
# 값 할당되기 이전에 출력하려 하니 오류가 난다
b = 10

def func_v3(a):
    print(a)
    print(b)
    b = 5

func_v3(5)
```

    5
    


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-4-ef0c8ce40129> in <module>
          7     b = 5
          8 
    ----> 9 func_v3(5)
    

    <ipython-input-4-ef0c8ce40129> in func_v3(a)
          4 def func_v3(a):
          5     print(a)
    ----> 6     print(b)
          7     b = 5
          8 
    

    UnboundLocalError: local variable 'b' referenced before assignment



```python
from dis import dis
print(dis(func_v3))
```

      5           0 LOAD_GLOBAL              0 (print)
                  2 LOAD_FAST                0 (a)
                  4 CALL_FUNCTION            1
                  6 POP_TOP
    
      6           8 LOAD_GLOBAL              0 (print)
                 10 LOAD_FAST                1 (b)
                 12 CALL_FUNCTION            1
                 14 POP_TOP
    
      7          16 LOAD_CONST               1 (5)
                 18 STORE_FAST               1 (b)
                 20 LOAD_CONST               0 (None)
                 22 RETURN_VALUE
    None
    

# Closure
- 반환되는 내부 함수에 대해서 선언된 연결을 가지고 참조하는 방식
- 반환 당시 함수 유효범위를 벗어난 변수 또는 메소드에 직접 접근이 가능하다


```python
a = 10
print(a + 10)
print(a + 100)
```

    20
    110
    


```python
# 결과를 누적할 수 없을까?
class Averager:
    def __init__(self):
        self._series = []

    def __call__(self, v):
        self._series.append(v)
        print("class >>> {} / {}".format(self._series, len(self._series)))
        return sum(self._series) / len(self._series)
```


```python
# 인스턴스 생성
avg_cls = Averager()
# 누적 확인
print(avg_cls(10))
print(avg_cls(20))
print(avg_cls(30))
```

    class >>> [10] / 1
    10.0
    class >>> [10, 20] / 2
    15.0
    class >>> [10, 20, 30] / 3
    20.0
    


```python
# 클로저 사용
def closure_avg1():
    # Free variable
    series = []
    # 클로저 영역
    def averager(v):
        # series = [] # Check
        series.append(v)
        print("def >>> {} / {}".format(series, len(series)))
        return sum(series) / len(series)
    return averager
```


```python
# averager()가 아닌 averager를 return한다.
# 즉, 함수를 리턴한다. 함수 실행 X
avg_closure1 = closure_avg1()
print(avg_closure1)
```

    <function closure_avg1.<locals>.averager at 0x0000023DBE876708>
    


```python
# 반환 당시 함수 유효범위를 벗어난 변수에 접근 가능
# averager() 함수 내에 series 변수를 선언하면 유지되지 않는다.
print(avg_closure1(15))
print(avg_closure1(35))
print(avg_closure1(40))
```

    def >>> [15] / 1
    15.0
    def >>> [15, 35] / 2
    25.0
    def >>> [15, 35, 40] / 3
    30.0
    


```python
def closure_avg2():
    # Free variable
    # series = []
    # 클로저 영역
    def averager(v):
        series = [] # Check
        series.append(v)
        print("def >>> {} / {}".format(series, len(series)))
        return sum(series) / len(series)
    return averager
```


```python
# 내부 변수가 유지되지 않는다
avg_closure2 = closure_avg2()
print(avg_closure2(15))
print(avg_closure2(35))
```

    def >>> [15] / 1
    15.0
    def >>> [35] / 1
    35.0
    


```python
print(dir(avg_closure1))
```

    ['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
    


```python
print(dir(avg_closure1.__code__))
```

    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'co_argcount', 'co_cellvars', 'co_code', 'co_consts', 'co_filename', 'co_firstlineno', 'co_flags', 'co_freevars', 'co_kwonlyargcount', 'co_lnotab', 'co_name', 'co_names', 'co_nlocals', 'co_stacksize', 'co_varnames']
    


```python
avg_closure1.__code__.co_freevars
```




    ('series',)




```python
# closure에도 함수처럼 속성들이 저장되어 있다
dir(avg_closure1.__closure__[0])
```




    ['__class__',
     '__delattr__',
     '__dir__',
     '__doc__',
     '__eq__',
     '__format__',
     '__ge__',
     '__getattribute__',
     '__gt__',
     '__hash__',
     '__init__',
     '__init_subclass__',
     '__le__',
     '__lt__',
     '__ne__',
     '__new__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     'cell_contents']




```python
print(avg_closure1.__closure__[0].cell_contents)
```

    [15, 35, 40]
    


```python
print(dir(avg_closure1.__closure__[0].cell_contents))
```

    ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
    

# 잘못된 클로저 사용 예


```python
def closure_avg3():
    # Free variable
    cnt = 0
    total = 0
    # 클로저 영역
    def averager(v):
        # series = [] # Check
        cnt += 1
        total += v
        return total / cnt

    return averager

avg_closure3 = closure_avg3()
print(avg_closure3(15))
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-22-38201db9fef4> in <module>
         13 
         14 avg_closure3 = closure_avg3()
    ---> 15 print(avg_closure3(15))
    

    <ipython-input-22-38201db9fef4> in averager(v)
          6     def averager(v):
          7         # series = [] # Check
    ----> 8         cnt += 1
          9         total += v
         10         return total / cnt
    

    UnboundLocalError: local variable 'cnt' referenced before assignment



```python
# 왜 오류가 날까
# cnt += 1 -> 클로저한테 외부 클로저 영역 freevar를 사용한단 것을 알려줘야 한다
def closure_avg3():
    # Free variable
    cnt = 0
    total = 0
    # 클로저 영역
    def averager(v):
        nonlocal cnt, total
        cnt += 1
        total += v
        return total / cnt

    return averager

avg_closure3 = closure_avg3()
print(avg_closure3(15))
print(avg_closure3(35))
```

    15.0
    25.0
    

# Decorator
- 중복 제거, 코드 간결
- 클로저보다 문법 간결 (파이썬에서)
- 조합해서 사용 용이  
- 단점
 - 디버깅 어려움
 - 에러의 모호함
 - 에러 발생 지점 추적 어려움
 - 해당 어려움들은 에디터의 도움으로 어느 정도 해결 가능


```python
import time
def perf_clock(func):
    def perf_clocked(*args):
        # 시작 시간
        st = time.perf_counter()
        result = func(*args)
        # 종료 시간
        et = time.perf_counter() - st
        # 함수명
        name = func.__name__
        # 매개변수
        arg_str = ','.join(repr(arg) for arg in args)
        # 출력
        print('[%0.5fs]%s(%s) -> %r'%(et, name, arg_str, result))
        return result
    return perf_clocked
```


```python
def time_func(seconds):
    time.sleep(seconds)
    
def sum_func(*numbers):
    return sum(numbers)

def fact_func(n):
    return 1 if n<2 else n * fact_func(n-1)
```


```python
# Non Decorator
non_deco1 = perf_clock(time_func)
non_deco2 = perf_clock(sum_func)
non_deco3 = perf_clock(fact_func)

# 스코프에서 함수가 유지되고 있음
print(non_deco1, non_deco1.__code__.co_freevars)
print(non_deco2, non_deco2.__code__.co_freevars)
print(non_deco3, non_deco3.__code__.co_freevars)
```

    <function perf_clock.<locals>.perf_clocked at 0x0000023DC0FBDA68> ('func',)
    <function perf_clock.<locals>.perf_clocked at 0x0000023DC0FBD318> ('func',)
    <function perf_clock.<locals>.perf_clocked at 0x0000023DC0FBD4C8> ('func',)
    


```python
non_deco1(2)
```

    [2.00015s]time_func(2) -> None
    


```python
non_deco2(10, 20, 30)
```

    [0.00000s]sum_func(10,20,30) -> 60
    




    60




```python
non_deco3(10)
```

    [0.00000s]fact_func(10) -> 3628800
    




    3628800




```python
# 데코레이터 사용
@perf_clock
def time_func(seconds):
    time.sleep(seconds)

@perf_clock
def sum_func(*numbers):
    return sum(numbers)

@perf_clock
def fact_func(n):
    return 1 if n < 2 else n * fact_func(n - 1)
```


```python
time_func(2)
```

    [1.99999s]time_func(2) -> None
    


```python
sum_func(10, 10)
```

    [0.00000s]sum_func(10,10) -> 20
    




    20




```python

```
