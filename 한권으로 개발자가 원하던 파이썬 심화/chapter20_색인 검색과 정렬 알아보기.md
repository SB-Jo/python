# 색인 검색
- Sequence 클래스 : 입력된 순서대로 원소를 저장한다
    - 문자열, 튜플, 리스트 등이 Sequence 클래스 상속해 구현한 클래스
- 순서를 가진다 = 원소마다 위치 레이블인 인덱스가 저장된다
- 색인 검색 = 인덱스로 검색


```python
import operator as op
# 인덱스 범위 벗어났을 때 예외 처리하기
def getitem_(obj, pos, default = object):
    try:
        result = op.getitem(obj, pos)
    except Exception as e:
        print(e)
        # 인덱스 범위 벗어났을 때 예외를 default에 저장한 값으로 처리
        result = default
    return result
```


```python
a = [1,2,3,4]
getitem_(a, 2)
```




    3




```python
getitem_(a,4)
```

    list index out of range
    




    object




```python
# 색인 연산으로 갱신하기
op.setitem(a, 2, 100)
a
```




    [1, 2, 100, 4]




```python
a.insert(4, 40)
a
```




    [1, 2, 100, 4, 40]



# 슬라이스 검색
슬라이스 = 시퀀스 내의 특정 범위를 부분으로 만드는 것  


```python
a = [0,1,2,3,4,5]
a1 = a[:]
print(a is a1)
print(id(a), id(a1))
```

    False
    2547216745032 2547216550280
    


```python
a1[1] = 999
print(a, a1)
```

    [0, 1, 2, 3, 4, 5] [0, 999, 2, 3, 4, 5]
    


```python
# None : 0번째 인덱스부터 처리
s = slice(None, 4)
a2 = a1[s]
a2
```




    [0, 999, 2, 3]



# 정렬


```python
s = "문자열 정리하기"
s.split()
```




    ['문자열', '정리하기']




```python
s = sorted(s.split())
s
```




    ['문자열', '정리하기']




```python
# 객체도 정렬 가능
class People:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
    def __repr__(self):
        return repr(("name:", self.name, "age:", self.age))
    
p = [People("Moon", 65), People("abc", 30), People("dahl", 45)]

sorted(p, key = lambda obj:obj.age)
```




    [('name:', 'abc', 'age:', 30),
     ('name:', 'dahl', 'age:', 45),
     ('name:', 'Moon', 'age:', 65)]




```python
class People2:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        
a = [People('qwe', 30), People('uio', 20)]
sorted(a, key = lambda obj:obj.age)
```




    [('uio', 20), ('qwe', 30)]




```python

```
