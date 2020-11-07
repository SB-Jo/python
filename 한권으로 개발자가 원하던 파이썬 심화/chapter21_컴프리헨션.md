# 리스트 컴프리헨션


```python
# map(함수, 리스트) : 함수를 받아서 리스트의 원소를 변형
# map은 반복자 객체를 생성한다. 반복자 객체를 자동으로 순환시켜 모든 원소 추출하려면 * 붙여야 한다
ll = [1,2,3,4,5]
m = map(lambda x: x*x, ll)
print(type(m))
print(m)
print(*m)
```

    <class 'map'>
    <map object at 0x000001E504315E88>
    1 4 9 16 25
    


```python
lh = [x*x for x in ll]
lh
```




    [1, 4, 9, 16, 25]




```python
# filter(함수, 리스트): filter클래스도 반복자 생성
f = filter(lambda x:x%2 == 0, lh)
print(f)
print(type(f))
print([*f])
```

    <filter object at 0x000001E504300348>
    <class 'filter'>
    [4, 16]
    


```python
lh1 = [x*x for x in ll if x%2 == 0]
lh1
```




    [4, 16]




```python
f1 = filter(lambda x:x%2==0, lh)
iter(f1)
print(next(f1))
print(next(f1))
```

    4
    16
    

# 딕셔너리와 집합 컴프리헨션


```python
s = {1,2,3,4,5}
m = map(lambda x:x*x, s)
print({*m})
```

    {1, 4, 9, 16, 25}
    


```python
ll = [('a', 1),('b',2)]
def t(x):
    x = list(x)
    x[1] = x[1] * x[1]
    x = tuple(x)
    return x

d = map(lambda x: t(x), ll)
[*d]
```




    [('a', 1), ('b', 4)]




```python
{x:y*y for x,y in ll}
```




    {'a': 1, 'b': 4}



# 컴프리헨션 주의점
- 컴프리헨션에 정의된 변수는 외부에서 사용불가. 컴프리헨션도 함수처럼 지역 이름공간 사용


```python
ll = [x for x in range(10)]
x
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-17-8a51449f1024> in <module>
          1 ll = [x for x in range(10)]
    ----> 2 x
    

    NameError: name 'x' is not defined



```python
# 모든 것을 다 실행한 후 지역 이름공간을 출력하기에 x는 최종값인 9로 출력
lll = [locals() for x in range(10)]
lll
```




    [{'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9},
     {'.0': <range_iterator at 0x1e50430abd0>, 'x': 9}]




```python
# 컴프리헨션 내에서 함수 처리
def power(x):
    return x*x

l = [power for x in range(3)]
print(l)
print(l[0](3))
```

    [<function power at 0x000001E5043CCC18>, <function power at 0x000001E5043CCC18>, <function power at 0x000001E5043CCC18>]
    9
    


```python
# 함수 실행
l2 = [power(x) for x in range(3)]
l2
```




    [0, 1, 4]



# 동적으로 반복자 처리
- map, filter는 동적 객체 핸들러(handler) 만들고 핸들러 사용해서 내부 값 가져온다.
- 반복자(iterator)클래스가 이를 지원


```python
# 반복자 클래스에는 __init__과 __next__가 필수
class Iterator:
    def __init__(self, iterable):
        # 초기화함수에서 반복되는 객체 받아 저장
        self.iterable = iterable
    
    # 반복자 객체 그대로 반환
    def __iter__(self):
        return self
    
    def __next__(self):
        if not self.iterable:
            raise StopIteration("데이터가 없습니다")
        return self.iterable.pop(0)
    
    
it = Iterator([1,2,3,4])
it.iterable
```




    [1, 2, 3, 4]




```python
next(it), next(it)
```




    (1, 2)




```python
next(it), next(it), next(it)
```


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-26-6d438fdb7d07> in <module>
    ----> 1 next(it), next(it), next(it)
    

    <ipython-input-24-8bc2d4725cd5> in __next__(self)
         11     def __next__(self):
         12         if not self.iterable:
    ---> 13             raise StopIteration("데이터가 없습니다")
         14         return self.iterable.pop(0)
         15 
    

    StopIteration: 데이터가 없습니다



```python
# 제네레이터 표현식 : 반복자를 처리하므로 표현식이 실행되면 반복자인 제네레이터 객체 생성
g = (x for x in range(1,5))
g
```




    <generator object <genexpr> at 0x000001E5043CD8C8>




```python
# 리스트컴프리헨션은 정적인 원소를 자동으로 만드는 구조
l = [x for x in range(1,5)]
l
```




    [1, 2, 3, 4]




```python

```
