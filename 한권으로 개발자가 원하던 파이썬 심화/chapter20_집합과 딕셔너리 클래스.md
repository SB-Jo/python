# dict


```python
d1 = {'a':1, 'b':2}
d1['c']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-1-fa2ac89c7221> in <module>
          1 d1 = {'a':1, 'b':2}
    ----> 2 d1['c']
    

    KeyError: 'c'



```python
# setdefault : 키가 없으면 키와 값을 저장해 결과 조회
d1.setdefault('c', 30)
```




    30




```python
d1
```




    {'a': 1, 'b': 2, 'c': 30}




```python
# OrderedDict : 순서 가지는 딕셔너리
import collections as cols
# OrderedDict는 dict 상속받는다
cols.OrderedDict.mro()
```




    [collections.OrderedDict, dict, object]




```python
# 모든 딕셔너리 클래스는 추상 클래스 Mapping 상속해서 구현
issubclass(cols.OrderedDict, cols.abc.Mapping)
```




    True




```python
cols.abc.Mapping.mro()
```




    [collections.abc.Mapping,
     collections.abc.Collection,
     collections.abc.Sized,
     collections.abc.Iterable,
     collections.abc.Container,
     object]




```python
d1 = {'a':1, 'b':2}
od = cols.OrderedDict(d1)
od
```




    OrderedDict([('a', 1), ('b', 2)])




```python
od['c'] = 100
od
```




    OrderedDict([('a', 1), ('b', 2), ('c', 100)])




```python
od.update({'d':100})
od
```




    OrderedDict([('a', 1), ('b', 2), ('c', 100), ('d', 100)])



# set


```python
# 집합은 해시로 구성하지만 키만 관리한다
s = {1,2,3,4}
f = frozenset(s)
type(s), type(f)
```




    (set, frozenset)




```python
s.add(5)
s
```




    {1, 2, 3, 4, 5}




```python
# set와 frozenset간 차집합 가능
s.difference(f), f.difference(s)
```




    ({5}, frozenset())




```python
# 교집합
s.intersection(f)
```




    {1, 2, 3, 4}




```python
# 합집합
s.union(f)
```




    {1, 2, 3, 4, 5}




```python
# issubset : 부분집합, issuperset : 상위집합
# s는 f의 부분집합이다 -> False
s.issubset(f), f.issubset(s)
```




    (False, True)




```python
s.issuperset(f), f.issuperset(s)
```




    (True, False)




```python
# remove() : 예외 발생
# discard() : 삭제할 원소가 없어도 예외 발생하지 않는다
ss = {4,4,4,4,5,6,7}
ss.discard(8)
```


```python
ss.remove(8)
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-28-c284a3b9929c> in <module>
    ----> 1 ss.remove(8)
    

    KeyError: 8



```python
# _update : 집합 연산에서 내부 원소 바꿀 때 사용
ss.difference_update(f)
ss
```




    {5, 6, 7}




```python
ss.intersection_update(f)
ss
```




    set()




```python
# isdisjoint() : 공집합 발생하는지 확인
sss = {5,7}
f.isdisjoint(sss), sss.isdisjoint(f)
```




    (True, True)




```python
# 멀티세트 처리 - 같은 원소를 몇 개 가졌는지 멀티세트로 확인 가능
cols.Counter.mro()
```




    [collections.Counter, dict, object]




```python
issubclass(cols.Counter, cols.abc.Mapping)
```




    True




```python
l = [1,1,1,3,3,4,4,4]
c = cols.Counter(l)
c
```




    Counter({1: 3, 3: 2, 4: 3})




```python
# Counter는 연산 가능
d = c + c
d
```




    Counter({1: 6, 3: 4, 4: 6})




```python
e = d - c
e
```




    Counter({1: 3, 3: 2, 4: 3})




```python
e == c
```




    True



# Heap 자료구조 처리하기


```python
class Heap:
    def __init__(self):
        self._dict = {}
        
    def empty(self):
        return True if len(self._dict) == 0 else False
    
    def pop(self, key):
        if self._dict.get(key, 0):
            return self._dict.pop(key)
        else:
            return 0
        
    def push(self, key, value):
        return self._dict.setdefault(key, value)
    
    def getHeap(self):
        return self._dict.copy()
```


```python
h1 = Heap()
h1.empty()
```




    True




```python
h1.push('가을', 100)
h1.push('겨울', 200)
h1.getHeap()
```




    {'가을': 100, '겨울': 200}




```python
# 변경 안됨 -> setdefault로 설계해서
h1.push('가을', 300)
h1.getHeap()
```




    {'가을': 100, '겨울': 200}




```python
h1.pop('겨울')
```




    200




```python
h1.pop('여름')
```




    0




```python

```
