# 외부 함수 안에 내부 함수 정의


```python
# 외부함수는 매개변수와 반환값을 처리하고
# 내부함수는 실제 기능을 처리하는 구성으로 함ㅅ ㅜㅅ너언
def outer(x,y):
    def inner():
        return x+y
    return inner()
```


```python
outer(5,5)
```




    10




```python
# 외부 함수 이름공간에는 2개의 매개변수와 내부 함수가 지역 이름공간에 존재
# 내부 함수 이름공간에는 외부 함수의 매개변수가 존재
def outer(x,y):
    def inner():
        print('inner local:', locals())
        return x+y
    print('outer local:', locals())
    return inner()

outer(5,5)
```

    outer local: {'inner': <function outer.<locals>.inner at 0x000001DFD4A064C8>, 'x': 5, 'y': 5}
    inner local: {'x': 5, 'y': 5}
    




    10



# 함수 이름공간의 스코프(scope) 이해하기
모듈은 전역 이름공간을 가지고 있어서 모듈 내부에 작성된 함수나 클래스가 이름공간에 정의된 변수를 참조할 수 있다.  
모듈에 정의된 함수는 지역 이름공간을 갖고 있고, 이 함수 내부에 또 함수를 정의하면 지역 이름공간이 만들어진다.  
이름공간에 있는 변수를 참조하여 범위를 정하는 규칙을 스코프라 한다.


```python
# 할당문은 지역 이름공간에 있는지 확인하고 실행
# 변수 사용하려면 먼저 변수에 값 할당하고 참조해야 함
def func(x,y):
    result = result + x+ y
    return result

func(5,5)
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-4-76119adf65af> in <module>
          3     return result
          4 
    ----> 5 func(5,5)
    

    <ipython-input-4-76119adf65af> in func(x, y)
          1 def func(x,y):
    ----> 2     result = result + x+ y
          3     return result
          4 
          5 func(5,5)
    

    UnboundLocalError: local variable 'result' referenced before assignment



```python
# 전역변수 : 모듈에 정의되어 전역 이름공간에 저장되는 변수
# 함수 내부에 같은 이름의 변수가 할당되어 지역이름공간 먼저 찾기에 에러 발생
result = 0
def func(x,y):
    result = result + x + y
    return result

func(5,5)
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-7-2a32827a5037> in <module>
          6     return result
          7 
    ----> 8 func(5,5)
    

    <ipython-input-7-2a32827a5037> in func(x, y)
          3 result = 0
          4 def func(x,y):
    ----> 5     result = result + x + y
          6     return result
          7 
    

    UnboundLocalError: local variable 'result' referenced before assignment



```python
# global 통해 전역변수임을 선언
result = 0
def func(x,y):
    global result
    result = result + x + y
    return result

func(5,5)
```




    10




```python
# 계층화된 구조
def outer():
    x = 0
    def inner():
        y = x+1
        return y
    return inner()

outer()
```




    1




```python
# 내부 함수에 있는 할당문에 사용한 변수 x가 지역이름공간에 없기에 외부함수 참조 불가
def outer():
    x = 0
    def inner():
        x = x+1
        return x
    return inner()

outer()
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-11-ccdc0193d039> in <module>
          7     return inner()
          8 
    ----> 9 outer()
    

    <ipython-input-11-ccdc0193d039> in outer()
          5         x = x+1
          6         return x
    ----> 7     return inner()
          8 
          9 outer()
    

    <ipython-input-11-ccdc0193d039> in inner()
          3     x = 0
          4     def inner():
    ----> 5         x = x+1
          6         return x
          7     return inner()
    

    UnboundLocalError: local variable 'x' referenced before assignment



```python
# 이를 해결하기 위해 nonlocal 사용
def outer2():
    x = 0
    def inner2():
        nonlocal x
        x = x + 1
        return x
    return inner2()

outer2()
```




    1




```python
def outer3():
    x = []
    def inner3():
        x.append(5)
        return x
    return inner3()

outer3()
```




    [5]




```python
def outer4():
    x = []
    def inner4():
        nonlocal x
        x = x.append(5)
        return x
    return inner4()

print(outer4())
```

    None
    

# 클로저 환경
클로저 : 내부 함수를 외부로 반환할 때 내부함수에서 외부함수의 지역변수를 계속 사용하는 환경  
클로저의 핵심은 내부 함수에서 사용하는 외부 함수의 변수인 "자유변수"



```python
def outer(x):
    def inner(y):
        return x+y
    return inner

a = outer(5)
# 변수 a에 내부 함수가 저장되었는지 확인
print(a.__name__)
# 클로저 환경 구성되면 자유변수에 대한 정보가 속성 __closure__에 튜플 형태로 저장
print(a.__closure__)
```

    inner
    (<cell at 0x000001DFD4A199A8: int object at 0x00007FF88ECAA210>,)
    


```python
# 내부에 저장된 것 확인
a.__closure__[0].cell_contents
```




    5




```python
a(10)
```




    15



# 부분함수 처리하기
부분함수 : 함수의 인자를 구분해 처리  
    - 외부 함수와 내부 함수를 사용해서 처리
    - 정의한 후에 호출해서 실행하거나, 최종인자가 전달될때까지 기다린 후 실행


```python
def outer1(x):
    def inner1(y):
        def inner2(z):
            return x+y+z
        return inner2
    return inner1

inner1 = outer1(5)
print(inner1.__name__)

inner2 = inner1(10)
print(inner2.__name__)
inner2(20)
```

    inner1
    inner2
    




    35



# 커링(currying) 처리하기
부분함수를 다른 말로 커링이라고도 한다.  
모듈 이용해 커링 처리하기


```python
import functools as fs
def add(x,y,z):
    return x+y+z

# partial() : 매개변수 고정
add = fs.partial(add, 1, 2)
add(3)
```




    6




```python
def add2(x,y,z):
    return x+y+z

add2 = fs.partial(add2, 1)
add2(2,3)

```




    6


