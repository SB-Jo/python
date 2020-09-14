# 변수가 객체를 바인딩한 후 자료 결정
프로그래밍언어(java, c 등) = 변수에 저장될 자료형 지정  
파이썬 = 변수는단순 값을 관리. 모든 값을 객체로 만들어서 사용하기 때문.
 - 변수에 특별한 자료형 지정하지 않아도 객체 정보 확인해서 클래스 정보를 처리
 - 객체 내부에 객체를 생성하는 클래스인 자료형을 가지고 있다
 - 동적 자료형(dynamic typing) : 객체 할당이 자료형 결정


```python
var = []
type(var)
```




    list




```python
# __class__ : 객체 내부 속성 조회
var.__class__
```




    list




```python
c = int('100')
type(c)
```




    int




```python
# 강한 자료형 -> 같은 클래스를 가지는 객체들끼리 연산 가능
100 + '100'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-6-f94ec5934fa9> in <module>
    ----> 1 100 + '100'
    

    TypeError: unsupported operand type(s) for +: 'int' and 'str'



```python
100 + int('100')
```




    200




```python
class A(object):
    def func(self):
        print('함수')

A.func
```




    <function __main__.A.func(self)>




```python
a = A()
# bound method -> 객체에 할당하고 호출하면 method가 된다
# 클래스에 정의된 함수가 변수에 bound될 때 method가 된다
a.func
```




    <bound method A.func of <__main__.A object at 0x0000019A7B3E5508>>



# 객체의 원소에 대한 변경 여부
- 파이썬은 객체 변경 가능 여부가 아주 중요

## 변경할 수 없는 자료형


```python
var = '문자열'
var[0] = '가'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-11-fc366e73a9bb> in <module>
          1 var = '문자열'
    ----> 2 var[0] = '가'
    

    TypeError: 'str' object does not support item assignment



```python
# 문자열은 변경 불가한 자료형 -> 문자열에 매소드 실행되면 새로운 문자열 객체 만들어서 반환된다
s = var.replace('문', '가')
print(var, s)
print(id(var), id(s))
```

    문자열 가자열
    1763004250416 1763004304240
    


```python
l = [1,2,3,4]
l[0]
```




    1




```python
l[0] = 100
l
```




    [100, 2, 3, 4]




```python
l.__setitem__(0, 999) # 0번째 인덱스의 값을 999로
l
```




    [999, 2, 3, 4]



# 다른 객체를 생성하는 형변환
- 파이썬에서 형변환은 새로운 클래스의 객체를 만드는 것
- 다른 클래스의 객체를 만들려면 클래스가 객체를 만들 때 필요한 매개변수에 인자를 전달해줘야 한다


```python
s = '100'
i = int(s)
type(i)
```




    int




```python
f = float(s)
type(f)
```




    float



# 변경 가능 여부 정보 확인하기
- 객체 변경 가능하다 = 변경할 수 있는 함수로 클래스를 정의했다는 뜻


```python
# 클래스에는 속성과 함수 등을 관리하는 이름공간 __dict__가 만들어진다
str.__dict__['__getitem__']
```




    <slot wrapper '__getitem__' of 'str' objects>




```python
# 문자열 객체는 변경 불가능 -> setitem 스폐셜 메소드가 정의되어 있지 않다
str.__dict__['__setitem__']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-24-b694cddd05b5> in <module>
          1 # 문자열 객체는 변경 불가능 -> setitem 스폐셜 메소드가 정의되어 있지 않다
    ----> 2 str.__dict__['__setitem__']
    

    KeyError: '__setitem__'



```python
list.__dict__['__getitem__']
```




    <method '__getitem__' of 'list' objects>




```python
list.__dict__['__setitem__']
```




    <slot wrapper '__setitem__' of 'list' objects>




```python
list.__dict__['__delitem__']
```




    <slot wrapper '__delitem__' of 'list' objects>




```python
lll = [1,2,3,4]
# 0번째 인덱스 값 가져오기
lll.__getitem__(0)
```




    1




```python
lll.__setitem__(0, 999)
lll
```




    [999, 2, 3, 4]




```python
lll[0] = 777
lll
```




    [777, 2, 3, 4]




```python

```
