# class ABC


```python
# class 선언
class VectorP(object):
    # private 선언
    def __init__(self, x, y):
        self.__x = float(x)
        self.__y = float(y)

    def __iter__(self):
        return (i for i in (self.__x, self.__y)) # Generator
    
    # getter
    @property
    def x(self):
        print('Called Property X')
        return self.__x
    
    @x.setter
    def x(self, v):
        print('Called Property X Setter')
        self.__x = v

    @property
    def y(self):
        return self.__y

    @y.setter
    def y(self, v):
        if v < -273:
            raise ValueError("Temperature below -273 is not possible")
        self.__y = v
```


```python
# 언더바 두개는 private - 직접 접근이 불가능하다
v = VectorP(20, 40)
print(v.__x, v.__y)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-10-736c9758329d> in <module>
          1 # 언더바 두개는 private - 직접 접근이 불가능하다
          2 v = VectorP(20, 40)
    ----> 3 print(v.__x, v.__y)
    

    AttributeError: 'VectorP' object has no attribute '__x'



```python
# 변수에 직접 접근이 아닌 proeprty 통해 접근
print(v.x)
```

    Called Property X
    20.0
    


```python
# Iter 확인
for val in v:
    print(val)
```

    20.0
    40.0
    


```python
# Setter 이용해 변경
v.x = 10
print(v.x)
```

    Called Property X Setter
    Called Property X
    10
    


```python
v.y = -280
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-15-055b003474b5> in <module>
    ----> 1 v.y = -280
    

    <ipython-input-9-374f1593048b> in y(self, v)
         27     def y(self, v):
         28         if v < -273:
    ---> 29             raise ValueError("Temperature below -273 is not possible")
         30         self.__y = v
    

    ValueError: Temperature below -273 is not possible



```python
# 내부에'_VectorP__x', '_VectorP__y' 생성되어 있음을 확인
print(dir(v), v.__dict__)
```

    ['_VectorP__x', '_VectorP__y', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'x', 'y'] {'_VectorP__x': 10, '_VectorP__y': 40.0}
    

# slot
- 파이썬 인터프리터에게 통보
- 해당 클래스가 가지는 속성을 제한한다
    - 파이썬 모든 클래스는 인스턴스 속성을 가진다
    - \__dict__ 통해서 이를 관리 -> 램 문제가 있는 자료구조
- slot 이용해 dict 속성 최적화 -> 다수 객체 생성 시 메모리 사용 공간 감소
- 해당 클래스에 만들어진 인스턴스 속성 관리에 딕셔너리 대신 Set형태를 사용


```python
# 성능 비교 (클래스 2개 선언)
class TestA(object):
    __slots__ = ('a',)

class TestB(object):
    pass

use_slot = TestA()
no_slot = TestB()
```


```python
# slot -> dict 대신 set 이용해서 없다
print(use_slot.__dict__)
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-18-3a5a3b4672dd> in <module>
    ----> 1 print(use_slot.__dict__)
    

    AttributeError: 'TestA' object has no attribute '__dict__'



```python
use_slot.a = 10
import timeit

# 측정 위한 함수 선언
def repeat_outer(obj):
    def repeat_inner():
        obj.a = 'test'
        del obj.a
    return repeat_inner

print('Slot : ', min(timeit.repeat(repeat_outer(use_slot), number=100000)))
print('No-Slot : ', min(timeit.repeat(repeat_outer(no_slot), number=100000)))
```

    Slot :  0.014328299999988303
    No-Slot :  0.016304900000022826
    

# 객체 슬라이싱


```python
class ObjectS:
    def __init__(self):
        self._numbers = [n for n in range(1, 100, 3)]
    
    def __len__(self):
        return len(self._numbers)

    def __getitem__(self, idx):
        return self._numbers[idx]

s = ObjectS()
```


```python
print(s.__dict__)
```

    {'_numbers': [1, 4, 7, 10, 13, 16, 19, 22, 25, 28, 31, 34, 37, 40, 43, 46, 49, 52, 55, 58, 61, 64, 67, 70, 73, 76, 79, 82, 85, 88, 91, 94, 97]}
    


```python
# class에 len 구현해놓아서 인스턴스에 바로 len 적용 가능
print(len(s))
```

    33
    


```python
print(s[1:10])
```

    [4, 7, 10, 13, 16, 19, 22, 25, 28]
    


```python
print(s[::10])
```

    [1, 31, 61, 91]
    

# 파이썬 추상 클래스
- 참고 : https://docs.python.org/3/library/collections.abc.html
- 자체적으로 객체 생성 불가
- 상속을 통해서 자식 클래스에서 인스턴스를 생성해야 함
- 개발과 관련된 공통된 내용(필드, 메소드) 추출 및 통합해서 공통된 내용으로 작성하게 하는 것


```python
# Sequence 상속 받지 않았지만, 자동으로 기능 작동
# 파이썬이 객체 전체를 자동으로 조사 -> 시퀀스 프로토콜 작동
# iter, contain 기능 구현하지 않아도 작동한다
class IterTestA():
    def __getitem__(self, idx):
        return range(1, 50, 2)[idx]
    
it1 = IterTestA()
print(it1[4])
print(it1[1:4])

# contain method 내부적으로 작동
print(3 in it1[1:10])

# iter 작동
print([i for i in it1])
```

    9
    range(3, 9, 2)
    True
    [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33, 35, 37, 39, 41, 43, 45, 47, 49]
    


```python
# Sequence 상속 - 요구사항인 추상메소드 모두 구현해야 작동
# len 없이 인스턴스화할 수 없다
from collections.abc import Sequence
class IterTestB(Sequence):
    def __getitem__(self, idx):
        return range(1, 50, 2)[idx] # range(1, 50, 2)
    
it2 = IterTestB()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-40-0059d4592414> in <module>
          5         return range(1, 50, 2)[idx] # range(1, 50, 2)
          6 
    ----> 7 it2 = IterTestB()
    

    TypeError: Can't instantiate abstract class IterTestB with abstract methods __len__



```python
class IterTestB(Sequence):
    def __getitem__(self, idx):
        return range(1, 50, 2)[idx] # range(1, 50, 2)
    
    def __len__(self, idx):
        return len(range(1, 50, 2)[idx])
    
it2 = IterTestB()
print(it2[4])
print(len(it2[:]))
print(len(it2[1:6]))
```

    9
    25
    5
    

# abc 활용 예제


```python
# 3.4버전 이하 -> abc.ABCMeta
import abc
class RandomMachine(abc.ABC):
    #__metaclass__ = abc.ABCMeta -> 이전 버젼
    
    # 추상 메소드
    @abc.abstractmethod
    def load(self, iterobj):
        '''Iterable 항목 추가'''
        
    @abc.abstractmethod
    def pick(self, iterobj):
        '''무작위 항목 뽑기'''
        
    def inspect(self):
        items = []
        while True:
            try:
                items.append(self.pick())
            except LookupError:
                break
        return tuple(sorted(items))
```


```python
import random
class CraneMachine(RandomMachine):
    def __init__(self, items):
        self._randomizer = random.SystemRandom()
        self._items = []
        self.load(items)
        
    def load(self, items):
        self._items.extend(items)
        self._randomizer.shuffle(self._items)
        
    def pick(self):
        try:
            return self._items.pop()
        except IndexError:
            raise LookupError('Empty Crane Box.')
            
    def __call__(self):
        return self.pick()
```


```python
# 서브 클래스 확인
print(issubclass(RandomMachine, CraneMachine))
print(issubclass(CraneMachine, RandomMachine))
```

    False
    True
    


```python
# 상속 구조 확인
print(CraneMachine.__mro__)
```

    (<class '__main__.CraneMachine'>, <class '__main__.RandomMachine'>, <class 'abc.ABC'>, <class 'object'>)
    


```python
cm = CraneMachine(range(1, 100))
print(cm._items)
```

    [26, 81, 74, 54, 92, 49, 21, 86, 65, 38, 2, 8, 18, 94, 33, 75, 71, 66, 23, 79, 53, 60, 15, 84, 61, 11, 77, 59, 10, 5, 4, 80, 47, 56, 44, 42, 1, 14, 83, 17, 7, 58, 40, 48, 19, 95, 3, 96, 34, 90, 28, 68, 25, 89, 72, 82, 69, 63, 88, 55, 62, 87, 73, 99, 30, 91, 37, 16, 97, 46, 31, 70, 98, 9, 20, 64, 29, 57, 12, 76, 27, 24, 45, 41, 6, 51, 67, 85, 78, 50, 39, 32, 13, 52, 93, 43, 35, 36, 22]
    


```python
print(cm.pick())
```

    26
    


```python
# callable 확인
print(cm())
```

    69
    


```python
# 부모에선 모든 자식에서 사용 가능한 메소드를 선언
# 자식에서 강제로 오버라이딩 해야 하는 추상 메소드 선언
# 강제로 오버라이딩해야 하는 메소드 이외의 부모 클래스 메소드들도 사용 가능
print(cm.inspect())
```

    (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99)
    


```python

```


```python

```
