# 자기 자신을 반복하는 재귀 함수
자기 자신을 호출해서 함수를 순환시키는 "재귀 함수"  
재귀 함수 사용하면 별도 순환문 사용하지 않고 반복해서 처리 가능  
재귀함수보다 순환문이 더 효율 좋다. 함수를 계속 만들어 처리하기 때문


```python
%%timeit
def list_sum(l):
    if len(l) == 1:
        return l[0]
    head, *tail = l
    return head + list_sum(tail)

list_sum([1,2,3,4,5])
```

    1.5 µs ± 29.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    


```python
%%timeit
def list_sum2(l):
    result = 0
    for i in l:
        result += i
    return result

list_sum2([1,2,3,4,5])
```

    349 ns ± 2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    

# 함수를 함수에 전달하는 합성 함수 처리


```python
def add(x,y):
    return x + y

import math
def power(func, *args, z=None):
    result = func(*args)
    if z:
        result = math.pow(result, z)
    return result

power(add, 1, 2, z=2)
```




    9.0



# 재귀 함수 실행 시 객체 이름공간 사용하기
함수가 정의되면 함수 내부에는 지역 이름공간이 생긴다.  
함수는 객체이므로 객체 이름공간도 만들어진다. 즉 함수는 두 개의 이름공간을 가진다.  


```python
# 리스트의 원소를 하나씩 꺼내서 함수 객체의 속성 result로 이동
def recusion(iterable):
    if not isinstance(iterable, list):
        return "처리할 수 없습니다."
    
    # 함수 객체에 "result" 속성이 있는지 확인. 없다면 속성 추가하고 빈 리스트 할당
    if not hasattr(recusion, "result"):
        recusion.result = []
        
    if len(iterable) == 1:
        recusion.result.append(iterable.pop())
        return None
    
    recusion.result.append(iterable.pop(0))
    return recusion(iterable)
```


```python
recusion([1,2,3,4])
recusion.result
```




    [1, 2, 3, 4]




```python
recusion("문자열 가능 여부")
```




    '처리할 수 없습니다.'




```python
recusion(3)
```




    '처리할 수 없습니다.'




```python

```
