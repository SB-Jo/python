# 삼항 연산 인라인 제어문으로 표시하기


```python
x = 10
print('x는 10보다 작습니다') if x < 10 else print('x는 10보다 크거나 같습니다')
```

    x는 10보다 크거나 같습니다
    


```python
a = 10
b = 5
c = a if a > b else b
c
```




    10



# 특정 값으로 조건을 판단하는 스위치 구문
- 특정 조건의 결과가 딕셔너리의 키로 정의되고 해당 값이 함수로 저장되어 특정 조건이 판별되면 딕셔너리 조회해서 함수 가져온다


```python
def func10():
    print(10)
    
def func100():
    print(100)
```


```python
switch = {'10' : func10, '100' : func100}

# 조건을 색인 연산에 전달 함수 반환 후 실행
switch['10']()
```

    10
    


```python
# 예외 방지 위해 get 메소드 사용
switch.get('100', func10)()
```

    100
    


```python
switch.get('1', func10)()
```

    10
    

# 특정 인덱스 정보로 검색하기
- 함수를 별도로 작성해 여러 개의 인덱스 정보 확인하기


```python
ll = [1,2,3,4,5,6]

def index(iterable, *args):
    result = []
    for i in args:
        if i < len(iterable):
            result.append(iterable[i])
        else:
            continue
    return result

# 1, 2, 5 번 인덱스 꺼내오기
print(index(ll, 1,2,5))
```

    [2, 3, 6]
    


```python
# operator - itemgetter : 특정 인덱스에 있는 값을 읽는 클래스
import operator as op
type(op.itemgetter)
```




    type




```python
inx = op.itemgetter(1,2,5)
print(type(inx))
print(inx(ll), type(inx(ll)))
```

    <class 'operator.itemgetter'>
    (2, 3, 6) <class 'tuple'>
    


```python

```
