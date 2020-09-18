# 반복할 수 있는 객체를 순환하는 for문 처리
- 원소를 여러 개 가지는 문자열, 리스트 등을 반복 순환


```python
# collections 모듈 내 추상클래스 관리하는 abc 모듈
# iterable = 반복형 추상 클래스. 원소를 여러 개 가진 정적인 객체 만든다

import collections.abc as cols
l = [1,2,3,4,5]
# 반복형 추상 클래스 상속했는지 확인하기
issubclass(type(l), cols.Iterable)
```




    True




```python
# Iterator - 반복자 추상 클래스
# Iterator - 동적 처리 위해 별도의 핸들러 만든 후 내부 원소를 하나씩 꺼내어서 처리
# 리시트는 반복자 추상 클래스를 상속받지 않았다
issubclass(type(l), cols.Iterator)
```




    False




```python
# range도 마찬가지로 Iterable은 상속했으나, Iterator는 상속받지 않았다
print(issubclass(range, cols.Iterable))
print(issubclass(range, cols.Iterator))
```

    True
    False
    

# 순환문에 else문 추가하기
- 정상적으로 순환이 처리되면 추가로 else문 실행
- 순환문이 break문으로 강제 종료되면 else문 실행하지 않는다


```python
result = 0
for i in range(1, 10):
    result += i
else:
    print('정상종료')
```

    정상종료
    


```python
for i in range(1, 10):
    result += i
    if result > 30:
        break
else:
    print('정상종료')
print(result)
```

    46
    

# 다양한 원소를 갖는 객체 원소 추출하기


```python
# enumerate 클래스로 객체 만들기
l = [1, 2, 3, 4]
for i, v in enumerate(l):
    print('index :', i, 'value :', v)
```

    index : 0 value : 1
    index : 1 value : 2
    index : 2 value : 3
    index : 3 value : 4
    


```python
# 순서 쌍을 만드는 zip 클래스의 객체 생성해서 순환
# 두 리스트 객체 원소로 순서 쌍 생성. -> 원소가 작은 리스트 기준으로 순서쌍 생성
ll = [4, 5, 6, 7, 8]
for i in zip(l, ll):
    print(i)
```

    (1, 4)
    (2, 5)
    (3, 6)
    (4, 7)
    


```python
# zip_longest : 남는 원소 그대로 출력하지만 부족한 원소는 None으로 처리
import itertools as it
for i in it.zip_longest(l, ll):
    print(i)
```

    (1, 4)
    (2, 5)
    (3, 6)
    (4, 7)
    (None, 8)
    


```python
# itertools.chain -> 두 개 리스트 연결
for i in it.chain(l, ll):
    print(i)
```

    1
    2
    3
    4
    4
    5
    6
    7
    8
    


```python
# itertools.combinations(list, number)
# list 중 원소number개 만큼 중복 없이 선택해서 경우의 수 만드는 조합으로 순환
# combinations는 순열 결과에서 순서를 무시한 것
for i in it.combinations(l, 3):
    print(i)
```

    (1, 2, 3)
    (1, 2, 4)
    (1, 3, 4)
    (2, 3, 4)
    


```python
# permutations -> 순서에 맞춰 순서 쌍 만들기
count = 0
for i in it.permutations(l, 3):
    print(i, end = ', ')
    count += 1
    if count % 5 ==0:
        print()
```

    (1, 2, 3), (1, 2, 4), (1, 3, 2), (1, 3, 4), (1, 4, 2), 
    (1, 4, 3), (2, 1, 3), (2, 1, 4), (2, 3, 1), (2, 3, 4), 
    (2, 4, 1), (2, 4, 3), (3, 1, 2), (3, 1, 4), (3, 2, 1), 
    (3, 2, 4), (3, 4, 1), (3, 4, 2), (4, 1, 2), (4, 1, 3), 
    (4, 2, 1), (4, 2, 3), (4, 3, 1), (4, 3, 2), 


```python

```
