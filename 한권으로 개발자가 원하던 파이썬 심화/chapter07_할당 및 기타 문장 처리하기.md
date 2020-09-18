파이썬에서 변수를 정의한다 = 변수 이름과 값으로 할당 문장을 작성한다  

# 변수에 값을 할당하는 문장
- 파이썬은 변수를 정의하면 이름공간(네임스페이스)에 변수 이름을 key로, 객체를 값으로 저장한다


```python
# globals() -> 딕셔너리로 관리하는 전역 이름공간 표시
var = 100
globals()['var']
```




    100




```python
globals()
```




    {'__name__': '__main__',
     '__doc__': 'Automatically created module for IPython interactive environment',
     '__package__': None,
     '__loader__': None,
     '__spec__': None,
     '__builtin__': <module 'builtins' (built-in)>,
     '__builtins__': <module 'builtins' (built-in)>,
     '_ih': ['', "var = 100\nglobals()['var']", 'globals()'],
     '_oh': {1: 100},
     '_dh': ['C:\\Users\\User\\Downloads\\python\\Untitled Folder'],
     'In': ['', "var = 100\nglobals()['var']", 'globals()'],
     'Out': {1: 100},
     'get_ipython': <bound method InteractiveShell.get_ipython of <ipykernel.zmqshell.ZMQInteractiveShell object at 0x00000223AF8A1908>>,
     'exit': <IPython.core.autocall.ZMQExitAutocall at 0x223af9d53c8>,
     'quit': <IPython.core.autocall.ZMQExitAutocall at 0x223af9d53c8>,
     '_': 100,
     '__': '',
     '___': '',
     '_i': "var = 100\nglobals()['var']",
     '_ii': '',
     '_iii': '',
     '_i1': "var = 100\nglobals()['var']",
     'var': 100,
     '_1': 100,
     '_i2': 'globals()'}




```python
# 함수도 전역이름공간에 저장
def func(x,y):
    return x,y

globals()['func']
```




    <function __main__.func(x, y)>




```python
# 전역이름공간에 저장된 함수 객체 실행하기
globals()['func'](5, 10)
```




    (5, 10)




```python
# 모듈에 클래스 정의하면 함수와 같은 방식으로
# 클래스 이름이 변수로 사용되고 클래스 객체가 값으로 할당
class Klass:
    def __init__(self, name):
        self.name = name

globals()['Klass']
```




    __main__.Klass




```python
k = globals()['Klass']("Jo")
k.name
```




    'Jo'




```python
# 객체의 이름공간 __dict__ 검색
k.__dict__
```




    {'name': 'Jo'}




```python
# 여러 값을 여러 변수에 할당하기
a, b, c = [1, 2, 3]
print(a, b, c, type(a), type(b), type(c))
```

    1 2 3 <class 'int'> <class 'int'> <class 'int'>
    


```python
d, e, f = '가을이'
print(d, e, f)
```

    가 을 이
    


```python
x, *y = 1, 2, 3
print(x, y)
```

    1 [2, 3]
    


```python
# 일대일 교환 -> 타 프로그램 언어처럼 임시변수 안 만들어도 가능하다
x, y = y, x
print(x, y)
```

    [2, 3] 1
    

# map 클래스 알아보기
- map 클래스에 함수와 리스트 전달하면 반복자 객체를 반환
- 반복자 객체를 다시 실행해야 전달받은 내부 원소들의 변형된 결과 확인 가능


```python
import math
m = map(math.sqrt, [1,2,3,4])
print(type(m))
```

    <class 'map'>
    


```python
# 첫 번째 방식
for i in m:
    print(i)
```

    1.0
    1.4142135623730951
    1.7320508075688772
    2.0
    


```python
# 두 번째 방식 
list(m)
```




    [1.0, 1.4142135623730951, 1.7320508075688772, 2.0]




```python
# 세 번째 방식
[*m]
```




    [1.0, 1.4142135623730951, 1.7320508075688772, 2.0]




```python

```
