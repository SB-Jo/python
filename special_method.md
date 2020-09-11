# Magic Method


```python
print(int)
```

    <class 'int'>
    


```python
# 모든 속성 및 메소드 출력
print(dir(int))
```

    ['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
    


```python
n = 100
print(n + 200)
print(n.__add__(200))
```

    300
    300
    


```python
n.__doc__
```




    "int([x]) -> integer\nint(x, base=10) -> integer\n\nConvert a number or string to an integer, or return 0 if no arguments\nare given.  If x is a number, return x.__int__().  For floating point\nnumbers, this truncates towards zero.\n\nIf x is not a number or if base is given, then x must be a string,\nbytes, or bytearray instance representing an integer literal in the\ngiven base.  The literal can be preceded by '+' or '-' and be surrounded\nby whitespace.  The base defaults to 10.  Valid bases are 0 and 2-36.\nBase 0 means to interpret the base from the string as an integer literal.\n>>> int('0b100', base=0)\n4"




```python
n.__bool__(), bool(n)
```




    (True, True)




```python
n * 100, n.__mul__(100)
```




    (10000, 10000)



## 클래스 예제


```python
class Student:
    def __init__(self, name, height):
        self._name = name
        self._height = height

    def __str__(self):
        return 'Student Class Info : {} , {}'.format(self._name, self._height)

    def __ge__(self, x):
        print('Called >> __ge__ Method.')
        if self._height >= x._height:
            return True
        else:
            return False

    def __le__(self, x):
        print('Called >> __le__ Method.')
        if self._height <= x._height:
            return True
        else:
            return False

    def __sub__(self, x):
        print('Called >> __sub__ Method.')
        return self._height - x._height
```


```python
s1 = Student('James', 181)
s2 = Student('Mie', 165)
```


```python
s1 >= s2
```

    Called >> __ge__ Method.
    




    True




```python
s1 <= s2
```

    Called >> __le__ Method.
    




    False




```python
s1 - s2
```

    Called >> __sub__ Method.
    




    16




```python
s2 - s1
```

    Called >> __sub__ Method.
    




    -16



## 클래스 예제 2


```python
class Vector(object):
    def __init__(self, *args):
        '''Create a vector, example : v = Vector(1,2)'''
        if len(args) == 0:
            self._x, self._y = 0, 0
        else:
            self._x, self._y = args

    def __repr__(self):
        '''Returns the vector infomations'''
        return 'Vector(%r, %r)' % (self._x, self._y)

    def __add__(self, other):
        '''Returns the vector addition of self and other'''
        return Vector(self._x + other._x, self._y + other._y)
    
    def __mul__(self, y):
        return Vector(self._x * y, self._y * y)

    def __bool__(self):
        return bool(max(self._x, self._y))
```


```python
v1 = Vector(3,5)
v2 = Vector(15, 20)
v3 = Vector()
```


```python
Vector.__init__.__doc__
```




    'Create a vector, example : v = Vector(1,2)'




```python
Vector.__repr__.__doc__
```




    'Returns the vector infomations'




```python
v1, v2, v3
```




    (Vector(3, 5), Vector(15, 20), Vector(0, 0))




```python
v1 + v2
```




    Vector(18, 25)




```python
v1 * 4
```




    Vector(12, 20)




```python
bool(v1), bool(v2)
```




    (True, True)




```python
bool(v3)
```




    False


