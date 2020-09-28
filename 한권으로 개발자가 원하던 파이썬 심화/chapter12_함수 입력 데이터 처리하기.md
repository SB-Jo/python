# 고정 및 가변 위치 인자 처리하기
- 고정 위치 인자 : 고정된 매개변수에 1대1로 매핑하는 인자를 전달
- 가변 위치 인자 : 하나의 매개변수에 여러 개의 인자를 전달


```python
# locals 통해 지역이름변수에 매개변수와 인자가 어떻게 매칭되는지 확인하기
def func_p(x, y, z):
    print('locals : ', locals())
    return x + y + z

func_p(1,2,3)
```

    locals :  {'x': 1, 'y': 2, 'z': 3}
    




    6




```python
# 매개변수에 초기값 설정
def func_d(x=1, y=1, z=1):
    print('locals : ', locals())
    return x + y + z

func_d()
```

    locals :  {'x': 1, 'y': 1, 'z': 1}
    




    3



## 가변 위치 인자 처리
- 함수 호출할 때 매개변수보다 많은 인자 전달 가능
- 2개의 변수에 5개의 원소 가진 리스트 할당하면 1대1 매핑이 되지 않아 예외 발생
- 이를 방지하기 위해 2개 변수 중 하나에 별표 붙여 여러 개 원소 받을 수 있다는 표시


```python
a, *b = [1,2,3,4,5]
print(a, type(a))
print(b, type(b))
```

    1 <class 'int'>
    [2, 3, 4, 5] <class 'list'>
    


```python
# args 매개변수에 5개 원소 가진 튜플이 활당된다
def func_a(*args):
    print("가변 : ", locals())
    result = 0
    for i in args:
        result += i
    return result

func_a(1,2,3,4,5)
```

    가변 :  {'args': (1, 2, 3, 4, 5)}
    




    15




```python
# **kwargs -> 가변 키워드 인자
def func_kw(**kwargs):
    print('가변', locals())
    result = 0
    for k, v in kwargs.items():
        result += v
    return result

func_kw(a = 1, b = 2)
```

    가변 {'kwargs': {'a': 1, 'b': 2}}
    




    3



# 매개변수 혼용 처리하기
- 순서 : 고정 위치, 가변 위치, 고정 키워드, 가변 키워드
- 고정위치와 고정키워드만 받고 싶을 때에는 가변 위치이 그냥 *만 넣는다


```python
def func_pak(x, y, *, a, b):
    print('locals:', locals())
    result = x + y + a + b
    return result

func_pak(1, 2, a=10, b=20)
```

    locals: {'x': 1, 'y': 2, 'a': 10, 'b': 20}
    




    33




```python
# a=10, b=20이 아니라 위치인자를 전달하면 오류 발생
func_pak(1,2,10,20)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-10-9669e4dc2297> in <module>
          1 # a=10, b=20이 아니라 위치인자를 전달하면 오류 발생
    ----> 2 func_pak(1,2,10,20)
    

    TypeError: func_pak() takes 2 positional arguments but 4 were given


# 색인 검색 함수를 메소드로 전환하기


```python
class Index:
    @staticmethod
    def index(iterable, *args):
        result = []
        for i in args:
            if i < len(iterable):
                result.append(iterable[i])
            else:
                continue
        return result
    
print(Index.index([1,2,3,4,5], 2,4))
```

    [3, 5]
    


```python
class Index_:
    def __init__(self, iterable):
        self.iterable = iterable
        
    def index(self, *args):
        result =[]
        for i in args:
            if i < len(self.iterable):
                result.append(self.iterable[i])
            else:
                continue
        return result
    
I = Index_([2,4,6,8,10])
print(I.index(2,4))
```

    [6, 10]
    


```python

```
