# 유리수와 정밀한 숫자 계산하는 모듈
파이썬에서는 정수, 실수, 복소수가 기본이고, 유리수와 십진수를 모듈로 제공한다


```python
# 유리수 처리하는 fractions 모듈
from fractions import Fraction
```


```python
# Fraction(분자, 분모)
f = Fraction(16, -10)
print(f)
# 분자(numerator), 분모(denominator)
print('분자 :', f.numerator)
print('분모 :', f.denominator)
```

    -8/5
    분자 : -8
    분모 : 5
    


```python
# from_float() : 유리수로 변환
import math
p = Fraction.from_float(math.pi)
# 가장 가까운 값 구하는 정수로 만ㄷㄴ다
print(p.numerator, p.denominator)
```

    884279719003555 281474976710656
    


```python
#limit_denominator() : 분모 범위를 특정 자릿수까지 제한
lim = Fraction.from_float(math.pi).limit_denominator(1000)
lim
```




    Fraction(355, 113)




```python
# 자연상수 e도 from_float 함수로 유리수로 변환 가능
e = Fraction.from_float(math.e)
print(e)

e_lim = Fraction.from_float(math.e).limit_denominator(1000)
print(e_lim)
```

    6121026514868073/2251799813685248
    1457/536
    


```python
# 파이썬은 무리수도 유리수로 변환 가능
q = Fraction.from_float(math.sqrt(2))
print(q)
q_lim = Fraction.from_float(math.sqrt(2)).limit_denominator(1000)
print(q_lim)
```

    6369051672525773/4503599627370496
    1393/985
    


```python
# decimal : 정밀도 높이기 위해 십진수 처리하는 모듈
import decimal as dec
# getcontext : 모듈 환경 확인하기 위한 함수
# prec : 정밀도 처리 속성 -> prec = 28이라면 소수점 28자리까지 계산
con = dec.getcontext()
con.prec
```




    28




```python
# 두 개의 Decimal 객체 만들어 나눗셈하면 정밀도가 적용
dec.Decimal(1) / dec.Decimal(7)
```




    Decimal('0.1428571428571428571428571429')




```python
pi = dec.Decimal(math.pi)
pi
```




    Decimal('3.141592653589793115997963468544185161590576171875')




```python
e = dec.Decimal(math.e)
e
```




    Decimal('2.718281828459045090795598298427648842334747314453125')




```python
print(math.sqrt(2))
print(dec.Decimal(math.sqrt(2)))
print(dec.Decimal(2).sqrt())
```

    1.4142135623730951
    1.4142135623730951454746218587388284504413604736328125
    1.414213562373095048801688724
    


```python
# 2의 제곱근
dec.Decimal(2) ** dec.Decimal('0.5')
```




    Decimal('1.414213562373095048801688724')




```python
# 특정 자릿수에서 내림
dec.Decimal('7.325').quantize(dec.Decimal('.01'), rounding=dec.ROUND_DOWN)
```




    Decimal('7.32')




```python
# 특정 자릿수에서 올림
dec.Decimal('7.325').quantize(dec.Decimal('.01'), rounding=dec.ROUND_UP)
```




    Decimal('7.33')




```python
# ceil 과 비교
print(dec.Decimal('7.325').quantize(dec.Decimal('1'), rounding=dec.ROUND_UP))
print(math.ceil(dec.Decimal('7.325')))
```

    8
    8
    


```python
print(dec.Decimal('7.325').quantize(dec.Decimal('1'), rounding=dec.ROUND_DOWN))
print(math.floor(dec.Decimal('7.325')))
```

    7
    7
    

# 연산자를 제공하는 모듈


```python
import operator as op

# floordiv() : 몫만 구하는 나눗셈(//)
print(op.floordiv(15, 10))
# truediv() : 몫과 나머지 구하는 나눗셈(/)
print(op.truediv(15, 10))
```

    1
    1.5
    


```python
# contains() : 포함 관계 처리
op.contains([1,2,3,4,5,10], 10)
```




    True




```python
# dot product
import numpy as np
a = np.array([1,2,3])
# 백터의 내적
print(op.matmul(a, a))
print(a @ a)
print(np.dot(a, a))
#
print(a * a)
```

    14
    14
    14
    [1 4 9]
    


```python

```
