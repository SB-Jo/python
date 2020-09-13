컨테이너(container)
 - 서로 다른 자료형 저장 가능[list, tuple. collections.deque]
 - Flat형 : 한 개의 자료형만 저장 [str, bytes, bytearray, array,array, memoryview]

가변
 - list, bytearray, array.array, memoryview, deque
 
불변
 - tuple, str, bytes


```python
#  Non comprehending list
chars = '!@#$%^&*()_+'
codes1 = []

# 유니코드로 변경
for s in chars:
    codes1.append(ord(s))

codes1
```




    [33, 64, 35, 36, 37, 94, 38, 42, 40, 41, 95, 43]




```python
# comprehending lists
codes2 = [ord(s) for s in chars]
codes2
```




    [33, 64, 35, 36, 37, 94, 38, 42, 40, 41, 95, 43]




```python
# comprehending lists + map, filter
codes3 = [ord(s) for s in chars if ord(s) > 40]
codes3
```




    [64, 94, 42, 41, 95, 43]




```python
codes4 = list(filter(lambda x: x>40, map(ord, chars)))
codes4
```




    [64, 94, 42, 41, 95, 43]



Generator
- 한 번에 한 개의 항목을 생성
- 메모리 유지하지 않는다 -> 속도가 빠르다


```python
chars = '!@#$%^&*()'
tuple_g = (ord(s) for s in chars)
tuple_g
```




    <generator object <genexpr> at 0x000001E58A0878C8>




```python
next(tuple_g)
```




    33




```python
dir(tuple_g)
```




    ['__class__',
     '__del__',
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
     '__iter__',
     '__le__',
     '__lt__',
     '__name__',
     '__ne__',
     '__new__',
     '__next__',
     '__qualname__',
     '__reduce__',
     '__reduce_ex__',
     '__repr__',
     '__setattr__',
     '__sizeof__',
     '__str__',
     '__subclasshook__',
     'close',
     'gi_code',
     'gi_frame',
     'gi_running',
     'gi_yieldfrom',
     'send',
     'throw']




```python
# Generator 예제 2
gen = ('%s' %c + str(n) for c in ['A', 'B', 'C', 'D'] for n in range(1, 11))
gen
```




    <generator object <genexpr> at 0x000001E58A0A61C8>




```python
for s in gen:
    print(s, end = ' ')
```

    A1 A2 A3 A4 A5 A6 A7 A8 A9 A10 B1 B2 B3 B4 B5 B6 B7 B8 B9 B10 C1 C2 C3 C4 C5 C6 C7 C8 C9 C10 D1 D2 D3 D4 D5 D6 D7 D8 D9 D10 


```python
# Array
# array('자료형', ) - 첫 인자로 자료형 받는다
import array
array_g = array.array('I', (ord(s) for s in chars))
array_g
```




    array('I', [33, 64, 35, 36, 37, 94, 38, 42, 40, 41])




```python
array_g.tolist()
```




    [33, 64, 35, 36, 37, 94, 38, 42, 40, 41]




```python

```
