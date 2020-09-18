# 단순 조건을 처리하는 단순 제어문


```python
if True:
    print('참인 값만 처리')
```

    참인 값만 처리
    


```python
if False:
    print('참인 값만 처리')
else:
    print('거짓인 값만 처리')
```

    거짓인 값만 처리
    


```python
# 조건식 판단하기 전에 예외발생이 먼저 처리된다
# 조건식 작성할 때 예외 발생하지 않도록 주의
if 1/0:
    print('참인 값만 처리')
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-3-f071979628eb> in <module>
          1 # 조건식 판단하기 전에 예외발생이 먼저 처리된다
          2 # 조건식 작성할 때 예외 발생하지 않도록 주의
    ----> 3 if 1/0:
          4     print('참인 값만 처리')
    

    ZeroDivisionError: division by zero



```python
if 100 is not 200:
    print('참인 값먼 처리')
```

    참인 값먼 처리
    

# 다양한 조건 판단하는 복합 제어문


```python
# False가 나온다 왜 그럴까?
# & -> 비트연산자. 7과 20을 이진수로 변환해 자리수에 1인 경우만 다시 숫자로 표시
# 즉,7 & b -> 4 -> a > 4 = True -> True >= 20 -> False
a = 10
b = 20
a > 7 & b >= 20
```




    False




```python
7 & b
```




    4




```python
# 연산자 우선순위 때문에 비교 연산자보다 비트 연산자가 먼저 실행된다
# 그러므로 괄호를 묶어 비교연산 부분 처리하기
(a > 7) & (b >=20)
```




    True



# 조건 연산자를 스폐셜 메소드로 처리하기


```python
1 <= 10
```




    True




```python
(1).__le__(10)
```




    True




```python
a, b = 10, 20
(a < b) and (b > a)
```




    True




```python
a.__lt__(b).__and__(b.__gt__(a))
```




    True




```python
# (a < c) and (c < b)
c = 15
a < c < b
```




    True




```python

```
