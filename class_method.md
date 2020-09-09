__인스턴스 메소드__
- 변수로 인스턴스화한 후 호출할 수 있는 메소드
- 각각의 인스턴스를 구분하는 고유한 정보값(id)을 처리해주긴 위한 기능 구현할 때 사용

__클래스 메소드__
- 클래스 변수는 공용이라 cls 받는다


```python
class Student(object):
    """
    Student Class
    Author : Jo
    Date : 2020.07.17
    Description : Class, Static, Insatnce Method
    """

    # Class 변수
    tuition_per = 1.0

    def __init__(self, id, first_name, last_name, email, grade, tuition, gpa):
        self._id = id
        self._first_name = first_name
        self._last_name = last_name
        self._email = email
        self._grade = grade
        self._tuition = tuition
        self._gpa = gpa

    # 인스턴스 메소드
    def full_name(self):
        return "{} {}".format(self._first_name, self._last_name)

    # 인스턴스 메소드
    def detail_info(self):
        return "Student Detail Info : {}, {}, {}, {}, {}, {}".format(
            self._id,
            # instance method 전달
            self.full_name(),
            self._email,
            self._grade,
            self._tuition,
            self._gpa,
        )

    # 인스턴스 메소드
    def get_fee(self):
        return "Before Tuition -> ID : {}, fee : {}".format(self._id, self._tuition)

    # 인스턴스 메소드
    def get_fee_culc(self):
        return "After tuition -> ID : {}, fee : {}".format(
            self._id, self._tuition * Student.tuition_per
        )

    def __str__(self):
        return "Student Info -> name : {} grade : {} email : {}".format(
            self.full_name(), self._grade, self._email
        )

    # 클래스 메소드
    # 클래스 변수는 공용이라 self가 아니라 cls 받는다
    # cls.tuition_per는 Student.tuition_per과 같다
    @classmethod
    def raise_fee(cls, per):
        if per <= 1:
            print("Please Enter 1 or More")
            return
        cls.tuition_per = per
        print("Succeed! Tuition Increased!")

    @classmethod
    def student_const(cls, id, first_name, last_name, email, grade, tuition, gpa):
        return cls(
            id, first_name, last_name, email, grade, tuition * cls.tuition_per, gpa
        )

    # Static Method
    @staticmethod
    def is_scholarship_st(inst):
        if inst._gpa >= 4.3:
            return "{} is a scholarship recipinet.".format(inst._last_name)
        return "Sorry. Not a scholarship recipinet."
```

__str__ 출력


```python
student_1 = Student(1, "Kim", "Sarang", "student1@nvaer.com", "1", 400, 3.5)
student_2 = Student(2, "Lee", "Myungho", "student2@daum.net", "2", 500, 4.3)
print(student_1)
print(student_2)
```

    Student Info -> name : Kim Sarang grade : 1 email : student1@nvaer.com
    Student Info -> name : Lee Myungho grade : 2 email : student2@daum.net
    

__전체 정보__


```python
print(student_1.detail_info())
print(student_2.detail_info())
```

    Student Detail Info : 1, Kim Sarang, student1@nvaer.com, 1, 400, 3.5
    Student Detail Info : 2, Lee Myungho, student2@daum.net, 2, 500, 4.3
    

__학비 정보(인상 전)__


```python
print(student_1.get_fee())
print(student_2.get_fee())
```

    Before Tuition -> ID : 1, fee : 400
    Before Tuition -> ID : 2, fee : 500
    

클래스 메소드 사용하지 않고 클래스 변수에 접근
- 직접 접근 권장하지 않는다
- 모든 사용자가 공유하는 클래스 변수는 캡슐화되어야 한다


```python
Student.tuition_per = 1.2
print(student_1.get_fee_culc())
print(student_2.get_fee_culc())
```

    After tuition -> ID : 1, fee : 480.0
    After tuition -> ID : 2, fee : 600.0
    

클래스 메소드 사용
- 클래스 메소드 사용하면 흐름을 만들어줄 수 있다(제어 가능)


```python
Student.raise_fee(1)
```

    Please Enter 1 or More
    


```python
Student.raise_fee(1.5)
print(student_1.get_fee_culc())
print(student_2.get_fee_culc())
```

    Succeed! Tuition Increased!
    After tuition -> ID : 1, fee : 600.0
    After tuition -> ID : 2, fee : 750.0
    

클래스 메소드 이용한 인스턴스 생성
- 왜 클래스 메소드를 이용해 인스턴스를 생성할까?
 - 제어가 가능하기 때문
 - 클래스 변수에 접근해 학비를 인상 후 내년도 입학 학생들에게 적용해보자.
이때 '내년도 입학 학생'을 인스턴스화하는 클래스 메소드를 따로 만들어주면 기본 학비에 인상된 학비를 넣어줄 수가 있다


```python
student_3 = Student.student_const(
    3, "Park", "Minji", "Student3@gmail.com", "3", 550, 4.5
)
student_4 = Student.student_const(
    4, "Cho", "Sunghan", "Student4@gmail.com", "4", 600, 4.1
)
```


```python
print(student_3)
print(student_4)
```

    Student Info -> name : Park Minji grade : 3 email : Student3@gmail.com
    Student Info -> name : Cho Sunghan grade : 4 email : Student4@gmail.com
    


```python
print(student_3.detail_info())
print(student_4.detail_info())
```

    Student Detail Info : 3, Park Minji, Student3@gmail.com, 3, 825.0, 4.5
    Student Detail Info : 4, Cho Sunghan, Student4@gmail.com, 4, 900.0, 4.1
    

스태틱 메소드
- 스태틱 메소드를 클래스 바깥에 별도로 만들어도 상관없다. 하지만 클래스와 밀접한 기능을 하고, 관계를 갖기에 클래스 안에 구현하는 것이 타당하다. 이때 만들어주는 것이 스태틱 메소드다.
- 데코레이터로 스태틱메소드를 구현해주면, cls, self 인자 없이 공통으로 사용 가능하다.


```python
# 클래스로 접근
print(Student.is_scholarship_st(student_1))
print(Student.is_scholarship_st(student_2))
print(Student.is_scholarship_st(student_3))
print(Student.is_scholarship_st(student_4))
```

    Sorry. Not a scholarship recipinet.
    Myungho is a scholarship recipinet.
    Minji is a scholarship recipinet.
    Sorry. Not a scholarship recipinet.
    


```python
# 인스턴스로 접근
print(student_1.is_scholarship_st(student_1))
```

    Sorry. Not a scholarship recipinet.
    


```python

```
