# 클래스와 객체 구조 알아보기
- 클래스는 다른 클래스를 상속할 수 있다. 구현 클래스에서 객체를 만들면 이 클래스와객체는 생성 관계가 유지되고 클래스가 상속한 모든 상위 클래스와 객체와의 생성 관계도 유지된다
- 클래스에 의해 생성된 객체는 항상 유일한 객체를 유지하기 위해 별도의 식별 레퍼런스를 갖는다. 같은 객체인지 아닌지 비교할 때 이 레퍼런스를 비교하면 된다

# 객체와 클래스의 관계 확인하기


```python
class Klass(object):
    pass

# 클래스는 메타 클래스로(type)로 만든다
isinstance(Klass, type)
```




    True




```python
print(isinstance(object, type)) # 모든 클래스는 메타 클래스로 생성 -> 모든 객체도 메타 클래스로 생성
print(isinstance(int, type)) # 정수 클래스도 메타 클래스로 생성
print(isinstance(dict, type))
```

    True
    True
    True
    

# 객체 내부 검사하기
- 객체는 클래스 상속 관계에 있는 이름공간을 모두 참조해서 사용 가능하다
- 객체에서 dir 함수로 조회하면 객체 이름공간보다 더 많은 정보 확인 가능


```python
class Klass(object):
    클래스속성 = 'Klass'
    def __init__(self, name):
        self.인스턴스속성 = name
        
k = Klass('달')
# 변수에 할당된 객체 조회 -> 클래스 이름 뒤에 레퍼런스가 16진수로 표시
k
```




    <__main__.Klass at 0x1572db2f188>




```python
id(k) # 레퍼런스를 16진수에서 10진수로 변환
```




    1473940484488




```python
count = 0
for i in dir(k):
    print(i, end = ', ')
    count += 1
    if count % 5 == 0:
        print()
```

    __class__, __delattr__, __dict__, __dir__, __doc__, 
    __eq__, __format__, __ge__, __getattribute__, __gt__, 
    __hash__, __init__, __init_subclass__, __le__, __lt__, 
    __module__, __ne__, __new__, __reduce__, __reduce_ex__, 
    __repr__, __setattr__, __sizeof__, __str__, __subclasshook__, 
    __weakref__, 인스턴스속성, 클래스속성, 


```python
Klass.__dict__
```




    mappingproxy({'__module__': '__main__',
                  '클래스속성': 'Klass',
                  '__init__': <function __main__.Klass.__init__(self, name)>,
                  '__dict__': <attribute '__dict__' of 'Klass' objects>,
                  '__weakref__': <attribute '__weakref__' of 'Klass' objects>,
                  '__doc__': None})




```python
k.__dict__
```




    {'인스턴스속성': '달'}



# 객체 레퍼런스 비교 방식


```python
# 객체 유일성 점검
i = 1000
a = 3000
print(i is a)
print(id(i), id(a))
```

    False
    1473939679504 1473939679600
    


```python
# 정수, 문자열, 튜플은 변경 불가능해서 같은 값은 하나의 객체만 만들어야 하지만 2개를 만들어서 처리한다
# immutable 예외
s1 = '가을이가'
s2 = '가을이가'
print(s1 is s2)
print(s1 == s2)
```

    False
    True
    


```python
# 문자열 생성자를 이용하면 먼저 만들어진객체를 인지해서 새로운 객체 만들지 않고, 하나의 객체 공유
sl1 = '가을이가'
sl2 = str(sl1)
sl1 is sl2
```




    True



# dataclass로 클래스 정의하기
- 타입 힌트만 작성해도 객체 속성 자동으로 생성


```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    
Person.__dict__['__init__']
```




    <function __main__.__create_fn__.<locals>.__init__(self, name: str, age: int) -> None>




```python
p = Person('moon', 52)
p.__dict__
```




    {'name': 'moon', 'age': 52}




```python
# 새로운 속성 외부에서 추가 가능
p.sex = 'male'
p.__dict__
```




    {'name': 'moon', 'age': 52, 'sex': 'male'}




```python
# init : 초기화함수 생성
# repr: repr 함수 생성
# eq : 객체 비교
# order : 비교 연산에대한 스폐셜 메소드
# unsafe_hash : hash 함수
# frozen : __setattr__,__delattr__ 함수 생성 결정
@dataclass(init=True, repr =False, eq=False,order= False,
          unsafe_hash = False,frozen =True)
class People:
    name: str
    age:int
        
pe = People('dahl', 52)
pe.__dict__
```




    {'name': 'dahl', 'age': 52}




```python
# frozen= True -> 객체의 속성값 변경 불가능
pe.age = 30
```


    ---------------------------------------------------------------------------

    FrozenInstanceError                       Traceback (most recent call last)

    <ipython-input-17-940a1ffbc77d> in <module>
          1 # frozen= True -> 객체의 속성값 변경 불가능
    ----> 2 pe.age = 30
    

    <string> in __setattr__(self, name, value)
    

    FrozenInstanceError: cannot assign to field 'age'



```python
@dataclass
class Product:
    '''Class for keeping track of an item in inventory'''
    name : str
    unit_price : float
    quantity_on_hand : int = 0
        
    def total_cost(self) -> float:
        return self.unit_price * self.quantity_on_hand
    
p = Product('bag', 1000.0, 10)
p.total_cost()
```




    10000.0




```python

```
