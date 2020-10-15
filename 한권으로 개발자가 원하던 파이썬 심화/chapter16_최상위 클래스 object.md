# 최상의 클래스 object
모든 객체지향 언어는 클래스 만들 때 최상위 클래스를 상속하는 구조로 되어 있다  
파이썬에서 최상위 클래스는 object이고, 클래스 정의할 때 상속하지 않아도 이 클래스를 기본으로 상속해서 사용한다  
최상위 클래스의 객체 생성하는 스폐셜 메소드를 사용해서 객체 생성이 가능하다

# 클래스 내부 속성 알아보기
object 클래스는 스폐셜 속성과 스폐셜 메소드만으로 구성


```python
print(dir(object))
```

    ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
    


```python
# 기본 클래스 리스트 안에 담기
ll = [str, int, list, tuple, type(None), object, type]
```


```python
# 최상위 클래스 object내의 내장 클래스로만 만든 객체를 가진 경우 추출
for i in dir(object):
    if (type(object.__getattribute__(object, i)) in ll):
        print(i)
```

    __class__
    __doc__
    


```python
type(object.__getattribute__(object,'__class__'))
```




    type




```python
type(object.__getattribute__(object, '__doc__'))
```




    str




```python
# 최상위 클래스 object는 객체 생성 가능
o = object()
# class 정보
print(o.__class__)
# 문서화 정보
print(o.__doc__)
```

    <class 'object'>
    The most base type
    

# 클래스 내부 메소드 알아보기


```python
# 내장 클래스가 아닌 경우에 추출하기
count = 0
for i in dir(object):
    if (type(object.__getattribute__(object, i)) not in ll):
        print(i, end = ', ')
        count += 1
        if count % 5 == 0:
            print()
```

    __delattr__, __dir__, __eq__, __format__, __ge__, 
    __getattribute__, __gt__, __hash__, __init__, __init_subclass__, 
    __le__, __lt__, __ne__, __new__, __reduce__, 
    __reduce_ex__, __repr__, __setattr__, __sizeof__, __str__, 
    __subclasshook__, 


```python
# 객체 생성자 : new
# 객체 초기화 : init
# builtin_function_or_method : 함수와 메소드가 가능한 구현체
type(object.__new__)
```




    builtin_function_or_method




```python
# wrapper_descriptor
type(object.__str__)
```




    wrapper_descriptor



# 문서화를 독테스팅(doctesting)으로 테스트하기
- 클래스, 함수 등은 문서화가능
- 문서화 내에 다양한 예제 작성해서 실행되는지 검토 가능한데, 이러한 방식으로 테스트하는 것을 독테스팅이라고 한다


```python
import doctest

def add(x,y):
    '''
    >>> add(5, 10)
    15
    '''
    return x+y

doctest.testmod()
```




    TestResults(failed=0, attempted=1)




```python
doctest.testmod(verbose=True)
```

    Trying:
        add(5, 10)
    Expecting:
        15
    ok
    1 items had no tests:
        __main__
    1 items passed all tests:
       1 tests in __main__.add
    1 tests in 2 items.
    1 passed and 0 failed.
    Test passed.
    




    TestResults(failed=0, attempted=1)




```python
%%writefile add_.py
def add_(x,y):
    '''
    >>> add_(5, 10)
    15
    '''
    return x + y
```

    Writing add_.py
    

주피터 노트북에서 모듈 실행하려면 쉘 명령어 직접 입력해 처리 가능  
!python -m doctest 파일명 -v


```python
!python -m doctest add_.py -v
```

    Trying:
        add_(5, 10)
    Expecting:
        15
    ok
    1 items had no tests:
        add_
    1 items passed all tests:
       1 tests in add_.add_
    1 tests in 2 items.
    1 passed and 0 failed.
    Test passed.
    


```python
%%writefile Foo.py
def foo(x):
    '''
    >>> foo(5)
    5
    '''
    return x

if __name__ in ("__main__", "__console__"):
    import doctest
    doctest.testmod(verbose=True)
```

    Writing Foo.py
    


```python
%run Foo.py
```

    Trying:
        foo(5)
    Expecting:
        5
    ok
    1 items had no tests:
        __main__
    1 items passed all tests:
       1 tests in __main__.foo
    1 tests in 2 items.
    1 passed and 0 failed.
    Test passed.
    

빈 클래스 정의한 후 함수에서 이 클래스의 객체 전달받아 리스트에 객체 넣어서 반환


```python
%%writefile unpredictable.py
class MyClass(object):
    pass

def unpredictable(obj):
    '''
    Returns a new list containing obj
    매개변수로 들어온 object를 리스트에 넣고 리턴해준다
    
    >>> unpredictable(MyClass())
    [<__main__.MyClass object at 0x...>]
    '''
    return [obj]

if __name__ in ("__main__", "__console__"):
    import doctest
    doctest.testmod(verbose=True)
```

    Writing unpredictable.py
    

위 모듈 실행하면 예외가 발생한다.  
실제 객체에 대한 레퍼런스가 다르기 때문이다.  
유일한 객체의 레퍼런스를 점검하지 않으려면 주석으로 생략 기호 넣어 처리해야 한다


```python
%run unpredictable.py
```

    Trying:
        unpredictable(MyClass())
    Expecting:
        [<__main__.MyClass.object at 0x....]
    **********************************************************************
    File "C:\Users\User\Downloads\python\한권으로 개발자가 원하던 파이썬 심화 AtoZ\unpredictable.py", line 9, in __main__.unpredictable
    Failed example:
        unpredictable(MyClass())
    Expected:
        [<__main__.MyClass.object at 0x....]
    Got:
        <__main__.MyClass object at 0x000002EDA3A69408>
    2 items had no tests:
        __main__
        __main__.MyClass
    **********************************************************************
    1 items had failures:
       1 of   1 in __main__.unpredictable
    1 tests in 3 items.
    0 passed and 1 failed.
    ***Test Failed*** 1 failures.
    


```python
%%writefile unpredictable.py
class MyClass(object):
    pass

def unpredictable(obj):
    '''
    Returns a new list containing obj
    매개변수로 들어온 object를 리스트에 넣고 리턴해준다
    
    >>> unpredictable(MyClass()) #doctest: +ELLIPSIS
    [<__main__.MyClass object at 0x...>]
    '''
    return [obj]

if __name__ in ("__main__", "__console__"):
    import doctest
    doctest.testmod(verbose=True)
```

    Overwriting unpredictable.py
    


```python
%run unpredictable.py
```

    Trying:
        unpredictable(MyClass()) #doctest: +ELLIPSIS
    Expecting:
        [<__main__.MyClass object at 0x...>]
    ok
    2 items had no tests:
        __main__
        __main__.MyClass
    1 items passed all tests:
       1 tests in __main__.unpredictable
    1 tests in 3 items.
    1 passed and 0 failed.
    Test passed.
    


```python

```
