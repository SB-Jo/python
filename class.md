# 클래스 기초

```python
class Student:
    '''
    Student Class
    Author : Kim
    Date : 2019.05.25
    '''

    student_count = 0

    def __init__(self, name, number, grade, details, email=None):
        # 인스턴스 변수
        self._name = name
        self._number = number
        self._grade = grade
        self._details = details
        self._email = email

        # 클래스 변수
        Student.student_count += 1

    def __str__(self):
        return "str {}".format(self._name)

    def __repr__(self):
        return "repr {}".format(self._name)

    def detail_info(self):
        print('Current Id: {}'.format(id(self)))
        print(
            "Student Detail Info : {} {} {}".format(
            self._name, self._email, self._details
            )
        )

    def __del__(self):
        Student.student_count -= 1
```

```python
studt1 = Student("Cho", 2, 3, {"gender": "Male", "score1": 65, "score2": 44})
studt2 = Student(
    "Chang", 4, 1, {"gender": "Female", "score1": 85, "score2": 74}, "stu2@naver.com"
)
```

## ** doc **

- 클래스에 적은 annotation 확인 가능

```python
Student.__doc__
```

    '\n    Student Class\n    Author : Kim\n    Date : 2019.05.25\n    '

```python
studt1
```

    repr Cho

## id()

- 객체를 인스턴스화할 때 모든 값을 같게해서 두 객체를 인스턴스화해도 id가 다르다

```python
print(id(studt1))
print(id(studt2))
```

    1739806071752
    1739806072072

## '==' & is

- '==' : 단순 값 비교
- 'is' : 값비교 + id 비교

```python
print(studt1._name == studt2._name)
print(studt1 is studt2)
```

    False
    False

## ** dict **

- 파이썬의 모든 객체는 ** dict ** 를 통해 네임스페이스 안의 속성을 불러올 수 있다

```python
print(studt1.__dict__)`
print(studt2.__dict__)
```

    {'_name': 'Cho', '_number': 2, '_grade': 3, '_details': {'gender': 'Male', 'score1': 65, 'score2': 44}, '_email': None}
    {'_name': 'Chang', '_number': 4, '_grade': 1, '_details': {'gender': 'Female', 'score1': 85, 'score2': 74}, '_email': 'stu2@naver.com'}

```python
print(dir(studt1))
```

    ['__class__', '__del__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_details', '_email', '_grade', '_name', '_number', 'detail_info', 'student_count']

```python
dir(studt1)[0]
```

    '__class__'

```python
students_list = []
students_list.append(studt1)
students_list.append(studt2)

for x in students_list:
    print(repr(x))
    print(x)
```

    repr Cho
    str Cho
    repr Chang
    str Chang

## class는 self가 없다

- 클래스 메소드를 클래스로 접근할 수 없다.
- 클래스는 인스턴스를 만들기 위한 틀

```python
Student.detail_info()
```

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-20-c624c0a8c7b5> in <module>
    ----> 1 Student.detail_info()


    TypeError: detail_info() missing 1 required positional argument: 'self'

```python
Student.detail_info(studt1)
```

    Current Id: 1739806951496
    Student Detail Info : Cho None {'gender': 'Male', 'score1': 65, 'score2': 44}

## 클래스가 다른 파일에 있는 경우

- 스크립트가 너무 길어 클래스 찾기 힘들거나
- 클래스가 다른 파일에 있는 경우
- ** class ** 를 통해서 클래스명 확인할 수 있다

```python
studt1.__class__
```

    __main__.Student

```python
# 동일 클래스이니 id도 같다
id(studt1.__class__) == id(studt2.__class__)
```

    True

## 인스턴스 변수

- 인스턴스 변수에 직접 접근해서 수정하는 방법 권장하지 않는다(PEP문법적으로 권장 X)
- 캡슐화(private) 유지해야 한다

```python
studt1._name = 'HAHAHA'
print(studt1._name)
```

## 클래스 변수

- 객체 및 클래스로 접근 가능

```python
studt1.student_count
```

    2

```python
Student.student_count
```

    2

## 클래스 네임 스페이스

- 인스턴스에는 클래스 변수가 존재하지 않는다. 하지만 인스턴스로 클래스 변수 접근이 가능하다. 왜?
  - 인스턴스 네임스페이스에 없으면 상위에서 검색한다
  - 인스턴스 검색 후 -> 상위(클래스 변수, 부모 클래스 변수) 검색

```python
Student.__dict__['student_count']
```

    2

```python
studt1.__dict__
```

    {'_name': 'Cho',
     '_number': 2,
     '_grade': 3,
     '_details': {'gender': 'Male', 'score1': 65, 'score2': 44},
     '_email': None}

```python

```
