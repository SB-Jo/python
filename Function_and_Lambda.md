```python
# 다중 리턴
def func_mul(x):
    y1 = x * 100
    y2 = x * 200
    y3 = x * 300
    return y1, y2, y3

val = func_mul(5)
print(val, type(val))
```

    (500, 1000, 1500) <class 'tuple'>
    


```python
val1, val2, val3 = func_mul(5)
print(val1, val2, val3)
```

    500 1000 1500
    

# args 이해하기
- 매개변수가 몇 개인지 모를 때
- 다양한 매개변수 형태를 받아서 함수 흐름이 바뀔 때


```python
def args_func(*args):
    for i, v in enumerate(args):
        print(i, v)
    print(args, type(args))

args_func('kim')
print()
args_func('kim', 'park')
```

    0 kim
    ('kim',) <class 'tuple'>
    
    0 kim
    1 park
    ('kim', 'park') <class 'tuple'>
    


```python
def args_func2(*args):
    print(*args)
    
args_func2('kim', 'park')
```

    kim park
    

# kwargs
- 딕셔너리가 인자로 넘어간다


```python
def kwargs_func(**kwargs):
    for k, v in kwargs.items():
        print(k, v)
    print(kwargs, type(kwargs))
    
kwargs_func(name1 = 'kim', name2 = 'park', name3 = 'lee')
```

    name1 kim
    name2 park
    name3 lee
    {'name1': 'kim', 'name2': 'park', 'name3': 'lee'} <class 'dict'>
    

# 중첩함수(클로저)


```python
def nested_func(num):
    def func_in_func(num):
        print(num)
    print('in func')
    func_in_func(num + 1000)
    
nested_func(10)
```

    in func
    1010
    

# lambda
- 메모리 절약, 가독성 향상, 코드 간결
- 함수는 객체 생성 -> 리소스(메모리) 할당
- 람다는 즉시 실행(Heap 초기화) -> 메모리 초기화



```python
lambda_mul_10 = lambda x:x * 10
print(lambda_mul_10(10))
```

    100
    


```python
def func_final(x, y, func):
    print(x + y + func(10))

func_final(10, 20, lambda_mul_10)
```

    130
    


```python

```
