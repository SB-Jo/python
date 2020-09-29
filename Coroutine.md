yield : 메인루틴 <->
- 코루틴 제어, 코루틴 상태, 양방향 값 전송
- 서브루틴 : 메인루틴에서 -> 리턴에 의해 호출 부분으로 돌아와 다시 프로세스
- 코루틴 : 루틴 실행 중 멈춤 가능 -> 특정 위치로 돌아갔다가 -> 다시 원래 위치로 돌아와 수행 가능 -> 동시성 프로그래밍 가능
- 코루틴 : 코루틴 스케쥴링 오버헤드 매우 적다 - 하나의 쓰레드에서 실행하기 때문
- 쓰레드 : 싱글쓰레드 -> 멀티쓰레드 = 복잡하다 -> 공유되는 자원에 교착 상태 발생할 가능성이 있다, 컨텍스트 스위칭 비용 발생, 자원 소비 가능성 증가

# 코루틴 예제 1


```python
def coroutine1():
    print('>>> coroutine started.')
    i = yield 10
    print('>>> coroutine received : {}'.format(i))

# 제네레이터 선언
c1 = coroutine1()
print(c1, type(c1))
```

    <generator object coroutine1 at 0x000002AF05162848> <class 'generator'>
    


```python
# yield 실행 전까지 진행
next(c1)
```

    >>> coroutine started.
    




    10




```python
# 기본으로 None 전달
# 값전송 - 더 이상 줄 값이 없어서 StopIteration
c1.send(100)
```

    >>> coroutine received : 100
    


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-32-e52290d462a5> in <module>
          1 # 기본으로 None 전달
          2 # 값전송
    ----> 3 c1.send(100)
    

    StopIteration: 



```python
# 잘못된 사용
# next() 통해 c2 호출후 멈춰놔야 값 전달 가능하다
c2 = coroutine1()
c2.send(100)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-4-9d3cf04240cc> in <module>
          2 # next() 통해 c2 호출후 멈춰놔야 값 전달 가능하다
          3 c2 = coroutine1()
    ----> 4 c2.send(100)
    

    TypeError: can't send non-None value to a just-started generator


# 코루틴 상태
- GEN_CREATED : 처음 대기 상태
- GEN_RUNNING : 실행 상태
- GEN_SUSPENDED : yield 대기 상태
- GEN_CLOSED : 실행 완료 상태


```python
def coroutine2(x):
    print('>>> coroutine started : {}'.format(x))
    # x: 메인루틴한테 전달할 값
    # y: 메인루틴이 코루틴한테 보내줘야 하는 값
    y = yield x
    print('>>> coroutine received : {}'.format(y))
    z = yield x + y
    print('>>> coroutine received : {}'.format(z))
    
c3 = coroutine2(10)

from inspect import getgeneratorstate
# 현재 만들어진 상태
print(getgeneratorstate(c3))
```

    GEN_CREATED
    


```python
# 10을 전달해주고 y값을 받기 위해 대기 상태
next(c3)
print(getgeneratorstate(c3))
```

    >>> coroutine started : 10
    GEN_SUSPENDED
    


```python
c3.send(15)
# 다시 z에서 대기중인 상태
print(getgeneratorstate(c3))
```

    >>> coroutine received : 15
    GEN_SUSPENDED
    

# 데코레이터 패턴 이용


```python
from functools import wraps

def coroutine(func):
    '''Decorator run until yield'''
    @wraps(func)
    def primer(*args, **kwargs):
        gen = func(*args, **kwargs)
        next(gen)
        return gen
    return primer

# decorator 패턴 통해 매번 next() 호출하는 번거로움 덜 수 있다
@coroutine
def sumer():
    total = 0
    term = 0
    while True:
        term = yield total
        total += term

su = sumer()
print(su.send(100))
print(su.send(40))
print(su.send(60))
```

    100
    140
    200
    

# 코루틴에 예외주기


```python
# Exception 클래스 상속받으면 해당 클래스를 예외로 사용할 수 있다
class SampleException(Exception):
    '''설명에 사용할 예외 유형'''

def coroutine_except():
    print('>> coroutine started.')
    try:
        while True:
            try:
                x = yield
            except SampleException:
                print('-> SampleException handled. Continuing')
            else:
                print('-> coroutine received : {}'.format(x))
    finally:
        print('-> coroutine ending')
        
exe_co = coroutine_except()
print(next(exe_co))
```

    -> coroutine ending
    >> coroutine started.
    None
    


```python
# send로 주기만 하고 , 받는게 없어서 None 출력
print(exe_co.send(10))
```

    -> coroutine received : 10
    None
    


```python
# 예외를 던졌지만 예외를 처리해주기 때문에 종료되지 않는다
print(exe_co.throw(SampleException))
```

    -> SampleException handled. Continuing
    None
    


```python
# 코루틴 종료
print(exe_co.close())
```

    -> coroutine ending
    None
    


```python
# 코루틴 예제(return)

def averager_re():
    total = 0.0
    cnt = 0
    avg = None
    while True:
        term = yield
        if term is None:
            break
        total += term
        cnt += 1
        avg = total / cnt
    return 'Average : {}'.format(avg)

avger2 = averager_re()
next(avger2)
avger2.send(10)
avger2.send(20)
avger2.send(30)

try:
    avger2.send(None)
except StopIteration as e:
    print(e)
```

    Average : 20.0
    

# yield from
- 현재는 await
- StopIteration을 자동으로 잡아준다


```python
def gen1():
    for x in 'AB':
        yield x
    for y in range(1,4):
        yield y

t1 = gen1()
for i in t1:
    print(i)
```

    A
    B
    1
    2
    3
    


```python
t2 = gen1()
print(list(t2))
```

    ['A', 'B', 1, 2, 3]
    


```python
# gen1과 동일한 결과값 나온다
def gen2():
    yield from 'AB'
    yield from range(1,4)
    
t3 = gen2()
print(list(t3))
```

    ['A', 'B', 1, 2, 3]
    


```python
def gen3_sub():
    print('Sub coroutine.')
    # x : send로 받을 값
    # 10 : 메인루틴한테 줄 값
    x = yield 10
    print('Recv : ', str(x))
    x = yield 100
    print('Recv : ', str(x))

# 메인루틴에서 코루틴을 관리할 함수 생성
def gen4_main():
    yield from gen3_sub()
    
t5 = gen4_main()
print(next(t5))
```

    Sub coroutine.
    10
    


```python
print(t5.send(7))
```

    Recv :  7
    100
    


```python
print(t5.send(77))
```

    Recv :  77
    


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-28-8be0cb769c3e> in <module>
    ----> 1 print(t5.send(77))
    

    StopIteration: 



```python

```
