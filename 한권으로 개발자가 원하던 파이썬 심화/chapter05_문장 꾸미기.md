# 멀티 라인문
- 변수에 표현식 할당할 때 연산자로 분리해 여러 줄에 쓰면, 첫번째 연산자 다음에 값이 없어 문법 예외가 발생한다


```python
a = 100 +
200 +
300
```


      File "<ipython-input-1-1066b9c74dc0>", line 1
        a = 100 +
                 ^
    SyntaxError: invalid syntax
    



```python
a = 100+\
200+\
300
```


```python
a
```




    600




```python
b = (100+200+300)
b
```




    600




```python
s = '''문자열
hello world'''
s
```




    '문자열\nhello world'



# 여러 문장을 한 줄에 작성하기
- 여러 문장을 한 줄에 쓸 때는 문장 구분을 위해 문장 끝에 세미콜론(;)을 쓴다


```python
a = 10; b = 20; c = 30
a, b, c
```




    (10, 20, 30)




```python
a, b, c = 10, 20, 30
a, b, c
```




    (10, 20, 30)




```python
for i in range(3): print(i); print('hello');
```

    0
    hello
    1
    hello
    2
    hello
    


```python
if a< b: print(c);print('hello')
```

    30
    hello
    


```python
# else문까지 한 줄에 사용하면 두 문장 구분할 수 없어 예외가 발생
if (a>b): princ(c); else: print('hello');
```


      File "<ipython-input-11-80ad3f36f862>", line 2
        if (a>b): princ(c); else: print('hello');
                               ^
    SyntaxError: invalid syntax
    



```python
if (a>b): print(c);
else: print('hello');
```

    hello
    

# 특정 숫자 임의로 추출하기


```python
import random
a = [x for x in range(1, 46)]
random.shuffle(a)
print(a)
```

    [33, 24, 35, 42, 34, 2, 29, 13, 6, 18, 10, 1, 26, 45, 12, 11, 39, 27, 4, 28, 8, 16, 22, 3, 25, 17, 30, 31, 38, 43, 23, 32, 5, 36, 40, 7, 15, 21, 9, 20, 44, 37, 19, 41, 14]
    


```python
# 특정 개수 원소 추출
random.choices(a, k=6)
```




    [42, 25, 35, 2, 31, 37]




```python
import numpy as np
# numpy 모듈
random.shuffle(a, random=np.random.random)
random.choices(a, k=6)
```




    [33, 29, 24, 12, 16, 23]




```python
np.random.shuffle(a)
random.choices(a, k=6)
```




    [17, 11, 41, 33, 20, 15]




```python

```
