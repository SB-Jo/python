# 예약어로 관리되는 객체


```python
# None은 NoneType 클래스로 만들었다
None.__class__
```




    NoneType




```python
dir(None.__class__)
```




    ['__bool__',
     '__class__',
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
     '__subclasshook__']




```python
set(dir(None.__class__)) - set(dir(object))
```




    {'__bool__'}




```python
# True, False는 bool클래스로 만들었다
type(True)
```




    bool




```python
set(dir(None.__class__)) - set(dir(int))
```




    set()




```python
# bool은 int 클래스를 상속했다
bool.__bases__
```




    (int,)



# 범위를 처리하는 클래스


```python
# slice, range는 클래스이다.
print(type(slice), type(range))
```

    <class 'type'> <class 'type'>
    


```python
s = slice(0, 1)
r = range(0, 1)

set(dir(s)) - set(dir(object))
```




    {'indices', 'start', 'step', 'stop'}




```python
# 반복행 객체의 특성 __len__, __getitem__
# 반복자로 변경할 수 있는 __iter__ 메소드 갖고 있다
set(dir(r)) - set(dir(object))
```




    {'__bool__',
     '__contains__',
     '__getitem__',
     '__iter__',
     '__len__',
     '__reversed__',
     'count',
     'index',
     'start',
     'step',
     'stop'}




```python

```
