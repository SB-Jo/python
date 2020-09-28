# 함수 정의하기
- 함수는 정의한 후에 사용해야 한다.
- 함수를 정의하는 것은 객체를 생성하는 것.
- 함수 객체는 여러 번 호출해서 사용 가능하다. 함수도 다른 객체와 마찬가지로 변수 할당, 매개변수의 인자, 반환값 등에 사용 가능

# 함수 객체를 만드는 함수 정의문
- 함수를 정의하면 함수 이름이 변수가 되어 함수 객체를 저장


```python
def func():
    pass

# 함수 객체 만들어지면 함수 이름은 속성 __name__에 저장
func.__name__
```




    'func'




```python
import types

# types 모듈 : 새로운 형의 동적 생성을 지원
# types.FunctionType -> 사용자 정의 함수, lambda표현식이 만든 함수의 형(type)
# 함수 정의문을 통해 만든 함수 객체가 실제 함수 클래스에 의해 생성되는 객체와 같다
type(func) == types.FunctionType
```




    True



# 파이썬 도움말
- 파이썬은 모듈, 함수, 클래스 정의할 때 기능에 관한 설명을 문자열로 작성하면 이를 __doc__ 속성에 자동으로 할당해서 문서화 작업 수행


```python
# help 클래스에 인자 전달하면 그 인자가 관리하는 문서화 속성(__doc__)을 조회해서 출력
help(int)
```

    Help on class int in module builtins:
    
    class int(object)
     |  int([x]) -> integer
     |  int(x, base=10) -> integer
     |  
     |  Convert a number or string to an integer, or return 0 if no arguments
     |  are given.  If x is a number, return x.__int__().  For floating point
     |  numbers, this truncates towards zero.
     |  
     |  If x is not a number or if base is given, then x must be a string,
     |  bytes, or bytearray instance representing an integer literal in the
     |  given base.  The literal can be preceded by '+' or '-' and be surrounded
     |  by whitespace.  The base defaults to 10.  Valid bases are 0 and 2-36.
     |  Base 0 means to interpret the base from the string as an integer literal.
     |  >>> int('0b100', base=0)
     |  4
     |  
     |  Methods defined here:
     |  
     |  __abs__(self, /)
     |      abs(self)
     |  
     |  __add__(self, value, /)
     |      Return self+value.
     |  
     |  __and__(self, value, /)
     |      Return self&value.
     |  
     |  __bool__(self, /)
     |      self != 0
     |  
     |  __ceil__(...)
     |      Ceiling of an Integral returns itself.
     |  
     |  __divmod__(self, value, /)
     |      Return divmod(self, value).
     |  
     |  __eq__(self, value, /)
     |      Return self==value.
     |  
     |  __float__(self, /)
     |      float(self)
     |  
     |  __floor__(...)
     |      Flooring an Integral returns itself.
     |  
     |  __floordiv__(self, value, /)
     |      Return self//value.
     |  
     |  __format__(self, format_spec, /)
     |      Default object formatter.
     |  
     |  __ge__(self, value, /)
     |      Return self>=value.
     |  
     |  __getattribute__(self, name, /)
     |      Return getattr(self, name).
     |  
     |  __getnewargs__(self, /)
     |  
     |  __gt__(self, value, /)
     |      Return self>value.
     |  
     |  __hash__(self, /)
     |      Return hash(self).
     |  
     |  __index__(self, /)
     |      Return self converted to an integer, if self is suitable for use as an index into a list.
     |  
     |  __int__(self, /)
     |      int(self)
     |  
     |  __invert__(self, /)
     |      ~self
     |  
     |  __le__(self, value, /)
     |      Return self<=value.
     |  
     |  __lshift__(self, value, /)
     |      Return self<<value.
     |  
     |  __lt__(self, value, /)
     |      Return self<value.
     |  
     |  __mod__(self, value, /)
     |      Return self%value.
     |  
     |  __mul__(self, value, /)
     |      Return self*value.
     |  
     |  __ne__(self, value, /)
     |      Return self!=value.
     |  
     |  __neg__(self, /)
     |      -self
     |  
     |  __or__(self, value, /)
     |      Return self|value.
     |  
     |  __pos__(self, /)
     |      +self
     |  
     |  __pow__(self, value, mod=None, /)
     |      Return pow(self, value, mod).
     |  
     |  __radd__(self, value, /)
     |      Return value+self.
     |  
     |  __rand__(self, value, /)
     |      Return value&self.
     |  
     |  __rdivmod__(self, value, /)
     |      Return divmod(value, self).
     |  
     |  __repr__(self, /)
     |      Return repr(self).
     |  
     |  __rfloordiv__(self, value, /)
     |      Return value//self.
     |  
     |  __rlshift__(self, value, /)
     |      Return value<<self.
     |  
     |  __rmod__(self, value, /)
     |      Return value%self.
     |  
     |  __rmul__(self, value, /)
     |      Return value*self.
     |  
     |  __ror__(self, value, /)
     |      Return value|self.
     |  
     |  __round__(...)
     |      Rounding an Integral returns itself.
     |      Rounding with an ndigits argument also returns an integer.
     |  
     |  __rpow__(self, value, mod=None, /)
     |      Return pow(value, self, mod).
     |  
     |  __rrshift__(self, value, /)
     |      Return value>>self.
     |  
     |  __rshift__(self, value, /)
     |      Return self>>value.
     |  
     |  __rsub__(self, value, /)
     |      Return value-self.
     |  
     |  __rtruediv__(self, value, /)
     |      Return value/self.
     |  
     |  __rxor__(self, value, /)
     |      Return value^self.
     |  
     |  __sizeof__(self, /)
     |      Returns size in memory, in bytes.
     |  
     |  __str__(self, /)
     |      Return str(self).
     |  
     |  __sub__(self, value, /)
     |      Return self-value.
     |  
     |  __truediv__(self, value, /)
     |      Return self/value.
     |  
     |  __trunc__(...)
     |      Truncating an Integral returns itself.
     |  
     |  __xor__(self, value, /)
     |      Return self^value.
     |  
     |  bit_length(self, /)
     |      Number of bits necessary to represent self in binary.
     |      
     |      >>> bin(37)
     |      '0b100101'
     |      >>> (37).bit_length()
     |      6
     |  
     |  conjugate(...)
     |      Returns self, the complex conjugate of any int.
     |  
     |  to_bytes(self, /, length, byteorder, *, signed=False)
     |      Return an array of bytes representing an integer.
     |      
     |      length
     |        Length of bytes object to use.  An OverflowError is raised if the
     |        integer is not representable with the given number of bytes.
     |      byteorder
     |        The byte order used to represent the integer.  If byteorder is 'big',
     |        the most significant byte is at the beginning of the byte array.  If
     |        byteorder is 'little', the most significant byte is at the end of the
     |        byte array.  To request the native byte order of the host system, use
     |        `sys.byteorder' as the byte order value.
     |      signed
     |        Determines whether two's complement is used to represent the integer.
     |        If signed is False and a negative integer is given, an OverflowError
     |        is raised.
     |  
     |  ----------------------------------------------------------------------
     |  Class methods defined here:
     |  
     |  from_bytes(bytes, byteorder, *, signed=False) from builtins.type
     |      Return the integer represented by the given array of bytes.
     |      
     |      bytes
     |        Holds the array of bytes to convert.  The argument must either
     |        support the buffer protocol or be an iterable object producing bytes.
     |        Bytes and bytearray are examples of built-in objects that support the
     |        buffer protocol.
     |      byteorder
     |        The byte order used to represent the integer.  If byteorder is 'big',
     |        the most significant byte is at the beginning of the byte array.  If
     |        byteorder is 'little', the most significant byte is at the end of the
     |        byte array.  To request the native byte order of the host system, use
     |        `sys.byteorder' as the byte order value.
     |      signed
     |        Indicates whether two's complement is used to represent the integer.
     |  
     |  ----------------------------------------------------------------------
     |  Static methods defined here:
     |  
     |  __new__(*args, **kwargs) from builtins.type
     |      Create and return a new object.  See help(type) for accurate signature.
     |  
     |  ----------------------------------------------------------------------
     |  Data descriptors defined here:
     |  
     |  denominator
     |      the denominator of a rational number in lowest terms
     |  
     |  imag
     |      the imaginary part of a complex number
     |  
     |  numerator
     |      the numerator of a rational number in lowest terms
     |  
     |  real
     |      the real part of a complex number
    
    


```python
def add_(x,y):
    '''이 함수는 두개의 매개변수를 덧셈하는 함수이다'''
    return x+y

add_.__doc__
```




    '이 함수는 두개의 매개변수를 덧셈하는 함수이다'



# 함수는 1급 객체이다
- 1급 객체는 변수, 매개변수, 반환값 등에 정의해서 사용 가능하다


```python
# 함수를 인자로 전달
def func(x, y):
    return x, y

def high_order(func, *args):
    return func(*args)

high_order(func, 1, 3)
```




    (1, 3)



# 함수 이름으로 접근해서 호출
- 함수는 어떻게 동작할까?
- 함수도 디스크립터 클래스이다. 디스크립터 클래스에는 반드시 정의해야 하는 스폐셜 메소드 \__get__이 정의되어 있다
- 함수 이름으로 조회하면 함수 객체를 자동으로 가져오기 위해 \__get__ 메소드가 자동으로 실행된다


```python
def add(x,y):
    return x+y

print(add.__get__)
```

    <method-wrapper '__get__' of function object at 0x000001CCB0714678>
    


```python
# 함수에 인자로 1을 넣고 실행하면 정수 객체 1의 메소드로 변환
# 이를 변수 a에 할당한 후 인자 2를 전달해서 실행
a = add.__get__(1)
a(2)
```




    3




```python
# __get__에 None 전달해서 실행하면 함수가 호출된다
# 두번째 인수는 함수를 가져오는데 아무런 역할 하지 않는다

print(add.__get__(None,add))
print(add.__get__(None,1))
```

    <function add at 0x000001CCB0714678>
    <function add at 0x000001CCB0714678>
    


```python
b = add.__get__(None,1)
b(3,4)
```




    7




```python

```
