파이썬의 중요한 핵심 프레임워크
- 시퀀스(Sequence), 반복(Iterator), 함수(Function), 클래스(Class)
- 객체 -> 파이썬의 데이터를 추상화
- 모든 객체는 id, type 함수 사용 가능하다 -> value를 갖고있다

# 네임드튜플


```python
# 튜플 사용해서 두점 사이 거리 구하기
pt1 = (1.0, 5.0)
pt2 = (2.5, 1.5)

from math import sqrt

line_leng1 = sqrt((pt2[0] - pt1[0]) ** 2 + (pt2[1] - pt1[1]) ** 2)
print(line_leng1)
```

    3.8078865529319543
    

다른 사람이 위 튜플을 이용하려면 pt1[0]과 pt2[0]이 x값임을 알아야 한다
- 이를 주석으로 달아 사용할 수 있다면 더 직관적이고 다른 사람들이 이용하기 쉬울 것이다
 -> NamedTuple 사용


```python
from collections import namedtuple

Point = namedtuple("Point", "x y")
pt1 = Point(1.0, 5.0)
pt2 = Point(2.5, 1.5)

line_leng2 = sqrt((pt2.x - pt1.x) ** 2 + (pt2.y - pt1.y) ** 2)
print(line_leng2)
print(line_leng1 == line_leng2)
```

    3.8078865529319543
    True
    

## 네임드튜플 선언 방법


```python
# 리스트로 선언
Point1 = namedtuple("Point", ["x", "y"])
Point2 = namedtuple("Point", "x, y")
# 하나의 단어면 상관없는데 띄어쓰기 있는 단어면 다른 단어로 인식하는 단점 - 위 방법 권장
Point3 = namedtuple("Point", "x y")
Point4 = namedtuple("Point", "x y x class", rename=True)
```


```python
print(Point1, Point2, Point3, Point4)
```

    <class '__main__.Point'> <class '__main__.Point'> <class '__main__.Point'> <class '__main__.Point'>
    


```python
# 객체 생성
# Point(x=10, y=20, _2=30, _3=40)
# rename=True -> 중복되는 값이나 예약어를 알아서 이름지어준다 - 혹시 모를 실수 대비
# Point4가 rename=False면 오류가 나타난다
p1 = Point1(x=10, y=35)
p2 = Point2(20, 40)
p3 = Point3(45, y=20)
p4 = Point4(10, 20, 30, 40)
print(p1, p2, p3, p4)
```

    Point(x=10, y=35) Point(x=20, y=40) Point(x=45, y=20) Point(x=10, y=20, _2=30, _3=40)
    


```python
# Dict를 unpacking 해서 사용하기
temp_dict = {"x": 75, "y": 55}
p5 = Point3(**temp_dict)
print(p5)
```

    Point(x=75, y=55)
    

## 네임드튜플 접근 방식


```python
# 인덱스 활용
print(p5[0], p5[1])
```

    75 55
    


```python
# 변수 접근 방식
print(p5.x, p5.y)
```

    75 55
    


```python
# unpacking 활용
a, b = p5
a + b
```




    130



## 네임드 튜플 메소드

-make() : 객체 생성


```python
temp = [52, 40]
p6 = Point1._make(temp)
print(p6)
```

    Point(x=52, y=40)
    

_fields() : 필드 네임 확인


```python
print(p1._fields, p2._fields, p3._fields)
```

    ('x', 'y') ('x', 'y') ('x', 'y')
    


```python
print(p4._fields)
```

    ('x', 'y', '_2', '_3')
    

_asdict(): OrderedDict로 변환
 - 정렬된 dict 형태


```python
print(p1._asdict(), p2._asdict(), p4._asdict())
```

    OrderedDict([('x', 10), ('y', 35)]) OrderedDict([('x', 20), ('y', 40)]) OrderedDict([('x', 10), ('y', 20), ('_2', 30), ('_3', 40)])
    


```python
# asdict -> dict
dict(p1._asdict())
```




    {'x': 10, 'y': 35}



_replace() : 수정된 '새로운' 객체 반환
 - id값이 바뀐다. 튜플은 불변형이라서


```python
p2, p2._replace(y=100)
```




    (Point(x=20, y=40), Point(x=20, y=100))



## 네임드튜플 실습

- 학생 전체 그룹 생성
- 반 20명, 4개의 반 (A, B, C, D)


```python
Classes = namedtuple('Classes', ['rank', 'number'])
```


```python
# 그룹 리스트 선언
numbers = [str(n) for n in range(1, 21)]
ranks = 'A B C D'.split()
```


```python
students = [Classes(rank, number) for rank in ranks for number in numbers]
print(students[:5])
```

    [Classes(rank='A', number='1'), Classes(rank='A', number='2'), Classes(rank='A', number='3'), Classes(rank='A', number='4'), Classes(rank='A', number='5')]
    


```python
print(students[-5:])
print(len(students))
```

    [Classes(rank='D', number='16'), Classes(rank='D', number='17'), Classes(rank='D', number='18'), Classes(rank='D', number='19'), Classes(rank='D', number='20')]
    80
    
