# 람다함수
- lambda 매개변수, 콜론, 표현식 순으로 작성
- 별도의 반환문이 없어 표현식이 결과를 그대로 반환


```python
lambda x,y : x+y
```




    <function __main__.<lambda>(x, y)>




```python
(lambda x,y: x+y)(5,5)
```




    10




```python
lam = lambda x,y: x+y
import types
print(type(lam))
print(type(lam) == types.LambdaType)
```

    <class 'function'>
    True
    


```python
lam.__name__
```




    '<lambda>'




```python
# 람다 함수인 람다 표현식에 pass가 올 수 없다.
(lambda x,y:pass)(5,5)
```


      File "<ipython-input-9-7127d19d3843>", line 2
        (lambda x,y:pass)(5,5)
                       ^
    SyntaxError: invalid syntax
    


# 람다 함수를 이용한 합성 함수 처리


```python
l = [1,2,3,4]
func = lambda x: x*x
(lambda func, obj:[func(x) for x in obj])(func, l)
```




    [1, 4, 9, 16]




```python
# 람다 함수를 전달받아 람다 함수 반환
c = lambda func:func
# 반환된 람다 함수 실행
c = c(func)
# 함수 실행
c(10)
```




    100




```python
# 외부 람다 함수가 함수를 전달받고, 내부 람다함수는 인자를 받고 전달받은 함수 실행
b = (lambda func: lambda *args, **kwargs: func(*args, **kwargs))
b = b(func)
b(10)
```




    100




```python
# 함수 인자를 분리해 부분 함수 만들어 처리하기
p = lambda x: lambda y: x+y
p(10)(30)
```




    40




```python
p = lambda x: lambda y: x+y
print(p)
p = p(10)
p
```

    <function <lambda> at 0x00000212F51C0048>
    




    <function __main__.<lambda>.<locals>.<lambda>(y)>




```python

```
