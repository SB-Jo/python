# 클래스 정의하기
- 파이썬에서 클래스는 객체를 만드는 툴(tool)이다

# 객체를 생성하는 클래스를 정의하는 문장
- 클래스 만드는 이유 : 클래스가 관리하는 영역과 객체를 만들어 사용하는 영역 구분하기 위해서
- 클래스나 객체는 자신이 사용하는 속성과 메소드를 가질 수 있다
- 객체를 계속 사용하려면 변수에 할당해야 하고 객체가 만들어지면 자신을 생성한 클래스 정보를 \__class__ 속성에 보관한다


```python
class Person(object):
    pass

p = Person()
# __main__ : 현재 작성되는 모듈이 이름이 __main__이라는 뜻
p.__class__
```




    __main__.Person



- 클래스가 정의되면 이름공간인 \__dict__가 만들어진다


```python
Person.__dict__
```




    mappingproxy({'__module__': '__main__',
                  '__dict__': <attribute '__dict__' of 'Person' objects>,
                  '__weakref__': <attribute '__weakref__' of 'Person' objects>,
                  '__doc__': None})



# 객체의 속성과 인스턴스 메소드 추가
- 파이썬에서 객체의 속성은 객체의 이름공간에서 관리하지만, 인스턴스 메소드는 클래스 이름공간에서 관리한다.


```python
class Person(object):
    def __init__(self, name):
        self.name = name
    
    def getName(self):
        return self.name
    
    def setName(self, name):
        self.name = name

p = Person("Dahl")
print("p_dict : ",p.__dict__)
print("class_dict : ", Person.__dict__)
```

    p_dict :  {'name': 'Dahl'}
    class_dict :  {'__module__': '__main__', '__init__': <function Person.__init__ at 0x000001B390DC7B88>, 'getName': <function Person.getName at 0x000001B390DC7C18>, 'setName': <function Person.setName at 0x000001B390DC7CA8>, '__dict__': <attribute '__dict__' of 'Person' objects>, '__weakref__': <attribute '__weakref__' of 'Person' objects>, '__doc__': None}
    


```python
# 객체가 클래스의 함수 이름에 접근하면 메소드가 된다
p.getName
```




    <bound method Person.getName of <__main__.Person object at 0x000001B390DF2C88>>



# 클래스 내부의 클래스 속성 정의하기
- 객체 속성과 클래스 속성 같은 이름으로 정의 가능하다


```python
class Person(object):
    name = 'Person Class Attr'
    def __init__(self, name):
        self.name = name
    
    def getName(self):
        return self.name
    
    def setName(self, name):
        self.name = name
        
p = Person('Dahl')
print(p.__dict__)
print('Object attr : ',p.name)
print('Class attr : ',Person.name)
```

    {'name': 'Dahl'}
    Object attr :  Dahl
    Class attr :  Person Class Attr
    

# Callable 알아보기
- 함수도 호출 연산자를 사용해 실행 가능하다
- 어떤 객체도 호출할 수 있게 만들수 있다
- 클래스 내부에 함수 객체 호출할 수 있는 스폐셜 메소드 \__call_이 구현


```python
import collections.abc as abc
# callable 상속하려면 __call__ 반드시 구현해야 한다
abc.Callable.__abstractmethods__
```




    frozenset({'__call__'})




```python

```
