# 배열
- 배열은 추상 클래스 Sequence를 상속받아 구현된 클래스
- 파이썬 내장 클래스에서 배열은 문자열, 리스트, 튜플 등 제공

# 튜플과 네임드 튜플


```python
t = 1,2,3,4
t1 = tuple((1,2,3,4))
t2 = tuple(t)
# 원소 값이 같으면 같은 객체이지만 내부적으로 별도의 객체 생성
print(t is t1)
# 같은 객체로 사용하고 싶으면 기존에 만든 튜플을 생성자에 넣어야 한다
print(t is t2)
```

    False
    True
    


```python
# count(x) : 원소x의 개수
print(t.count(3))
# index(x) : x의 인덱스 
print(t.index(3))
```

    1
    2
    


```python
# tuple은 변경, 삭제, 추가 불가능
tuple.__setitem__
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-4-abf6e5fcdb7c> in <module>
          1 # tuple은 변경, 삭제, 추가 불가능
    ----> 2 tuple.__setitem__
    

    AttributeError: type object 'tuple' has no attribute '__setitem__'



```python
# named tuple : tuple의 각 원소에 이름 붙이고 이름으로 접근 가능
import collections as cols
# namedtuple("튜플이름", "필드 이름")
NamedTuple = cols.namedtuple("NamedTuple", "x, y, z")

# type통해 내부에 클래스가 만들어진 것 알 수 있다
type(NamedTuple)
```




    type




```python
NamedTuple
```




    __main__.NamedTuple




```python
# 튜플 클래스로 변수 x 조회하면 property 객체로 출력
NamedTuple.x
```




    <property at 0x2408dfc4f98>




```python
# NamedTuple 클래스에 3개 인자 전달해 새로운 네임드 튜플 객체 생성
nt = NamedTuple(1,2,3)
print(nt)
print(nt.x)
```

    NamedTuple(x=1, y=2, z=3)
    1
    


```python
# typing : 다양한 타입 지정 가능한 모듈
# typing 모듈 안에 NamedTuple 클래스가 있는데, 이를 상속해 클래스 생성
import typing
class Point(typing.NamedTuple):
    x: int
    y: int
    z: int
        
# 속성이 순서를 가져야 하므로 OrderedDict 클래스의 객체
Point.__annotations__
```




    OrderedDict([('x', int), ('y', int), ('z', int)])




```python
Point.x
```




    <property at 0x2408dfdc138>




```python
p = Point(1,2,3)
print(p)
print(p.x)
```

    Point(x=1, y=2, z=3)
    1
    


```python
# 색인 연산도 가능
p[0]
```




    1




```python
# 네임드튜플도 튜플 클래스 상속해 만든 것이므로 네임드 튜플 객체와 생성관계가 성립
isinstance(p, tuple)
```




    True



# 리스트
리스트는 object 클래스를 자료형으로 인식해서 모든 파이썬 객체를 원소로 가질 수 있고 원소를 변경, 삭제, 추가할 수 있는 변경 가능한 클래스  



```python
# mutable한 객체는 항상 새로운 객체 만든다
a = [1,2,3,4]
a1 = list([1,2,3,4])
a2 = list(a)
print(a is a1)
print(a is a2)
```

    False
    False
    


```python
a.append(a2)
a
```




    [1, 2, 3, 4, [1, 2, 3, 4]]




```python
# remove(값)
a.remove(4)
a
```




    [1, 2, 3, [1, 2, 3, 4]]




```python
a3 = a1 + a2
a3
```




    [1, 2, 3, 4, 1, 2, 3, 4]




```python
a1.extend(a2)
a1
```




    [1, 2, 3, 4, 1, 2, 3, 4]




```python
# 리스트 객체의 모든 원소 삭제
a1.clear()
a1
```




    []




```python
# insert(인덱스,값)
a1.insert(0,1)
a1.insert(0,2)
a1
```




    [2, 1]



# 얕은 복사와 깊은 복사


```python
# 새로운 변수에 기존 변수 할당하면 동일한 객체의 레퍼런스가 복사
# 같은 객체를 2개의 변수에서 접근 가능
a = [1,2,3,4]
a1 = a
id(a), id(a1)
```




    (2476283298760, 2476283298760)




```python
a2 = a.copy()
print(a is a2)
print(id(a), id(a2))
```

    False
    2476283298760 2476242977288
    


```python
a.append(a2)
a
```




    [1, 2, 3, 4, [1, 2, 3, 4]]




```python
a3 = a.copy()
a3
```




    [1, 2, 3, 4, [1, 2, 3, 4]]




```python
# 원본도 같이 변경되었다
# 얕은복사(copy())는 중첩된 리스트는 복사하지 않기 때문
a3[4][0] = 999
a, a3
```




    ([1, 2, 3, 4, [999, 2, 3, 4]], [1, 2, 3, 4, [999, 2, 3, 4]])




```python
# 중첩된 리스트의 레퍼런스 확인
id(a[4]), id(a3[4])
```




    (2476242977288, 2476242977288)




```python
# deepcopy() 중첩 리스트 새로 만드는 깊은복사
import copy
a4 = copy.deepcopy(a3)
a4[4][1] = 888
a4, a3
```




    ([1, 2, 3, 4, [999, 888, 3, 4]], [1, 2, 3, 4, [999, 2, 3, 4]])




```python

```
