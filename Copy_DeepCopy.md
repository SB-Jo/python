# Python Object Reference
- 파이썬 객체 참조


```python
print(dir())
```

    ['In', 'Out', '_', '__', '___', '__builtin__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__', '_dh', '_i', '_i1', '_ih', '_ii', '_iii', '_oh', 'exit', 'get_ipython', 'quit']
    


```python
# id vs __eq__(==) 증명
x = {'name': 'kim', 'age': 33, 'city': 'Seoul'}
y = x

print(id(x), id(y))
print(x == y)
print(x is y)
```

    2402082023608 2402082023608
    True
    True
    


```python
# 여러 개의 변수가 하나를 참조하고 있다
x['class'] = 10
print(x, y)
print(x==y)
print(x is y)
```

    {'name': 'kim', 'age': 33, 'city': 'Seoul', 'class': 10} {'name': 'kim', 'age': 33, 'city': 'Seoul', 'class': 10}
    True
    True
    


```python
# 새로 값을 할당하면 새로운 id
z = {'name': 'kim', 'age': 33, 'city': 'Seoul', 'class': 10}
# id 값이 다르기 때문에 x is z -> False
print(x is z)
# 값이 같으므로 x == z -> True
print(x == z)
```

    False
    True
    True
    


```python
# 튜플 불변성의 비교
tuple1 = (10, 15, [100, 1000])
tuple2 = (10, 15, [100, 1000])

print(id(tuple1), id(tuple2))
print(tuple1 is tuple2)
print(tuple1 == tuple2)
print(tuple1.__eq__(tuple2))
```

    2402081510648 2402081510888
    False
    True
    True
    

# Copy, Deepcopy


```python
# copy
tl1 = [10, [100, 105], (5, 10, 15)]
tl2 = tl1
tl3 = list(tl1)

print(tl1 == tl2)
print(tl1 is tl2)
# 리스트 생성자로 생성 -> 새로운 생성자를 이용해 값을 할당하면 새로운 id
print(tl1 == tl3)
print(tl1 is tl3)
```

    True
    True
    True
    False
    


```python
tl1.append(1000)
tl1[1].remove(105)

# tl1, tl2 바뀌고 tl3는 새로운 객체이기 때문에 바뀌지 않는다
print(tl1)
print(tl2)
print(tl3)
```

    [10, [100], (5, 10, 15), 1000]
    [10, [100], (5, 10, 15), 1000]
    [10, [100], (5, 10, 15)]
    


```python
print('Befor Tuple ID:',id(tl1[2]))
tl1[1] += [110, 120]
tl1[2] += (110, 120)

print(tl1)
print(tl2)
# 튜플 재할당 객체 새로 생성 -> 튜플은 불변형이기에 재할당 시 객체를 새로 생성한다
# 리스트 안에 튜플이 들어가면 불안정할 수 있다. 새로운 객체가 생성됨은 메모리 사용이 많아짐을 뜻함
print('After Tuple ID:', id(tl1[2]))
print(tl3)
```

    Befor Tuple ID: 2402081511688
    [10, [100, 110, 120], (5, 10, 15, 110, 120), 1000]
    [10, [100, 110, 120], (5, 10, 15, 110, 120), 1000]
    After Tuple ID: 2402081610536
    [10, [100, 110, 120], (5, 10, 15)]
    


```python
# 장바구니 class
class Basket:
    def __init__(self, products=None):
        if products is None:
            self._products = []
        else:
            self._products = list(products)
    def put_prod(self, prod_name):
        self._products.append(prod_name)
        
    def del_prod(self, prod_name):
        self._products.remove(prod_name)
        
import copy
basket1 = Basket(['Apple', 'Bag', 'TV', 'Snack', 'Water'])
basket2 = copy.copy(basket1)
basket3 = copy.deepcopy(basket1)

# 객체를 copy(얕은복사)로 하니 일단 id는 다르다
print(id(basket1), id(basket2), id(basket3))
# 그렇다면 내부 속성은? 얕은복사를 하면 안의 속성들은 같은 id값 갖고 있다
print(id(basket1._products), id(basket2._products), id(basket3._products))
```

    2402114351496 2402114351432 2402114354120
    2402114353864 2402114353864 2402114274760
    


```python
basket1.put_prod('Orange')
basket2.del_prod('Snack')

# 같은 리스트를 참조하기 때문에 basket1에서도 snack이 제거되고, basket2에도 Orange가 추가됨
print(basket1._products)
print(basket2._products)
print(basket3._products)
```

    ['Apple', 'Bag', 'TV', 'Water', 'Orange']
    ['Apple', 'Bag', 'TV', 'Water', 'Orange']
    ['Apple', 'Bag', 'TV', 'Snack', 'Water']
    

# 함수 매개변수 전달 사용법


```python
def mul(x,y):
    x += y
    return x

# 불변형은 함수에 매개변수로 전달해도 원본 변경되지 않는다
x = 10
y = 5
print(mul(x,y), x, y)

# 가변형은 원본 데이터가 변경된다. a가 변경된다. 함수 안에서 a += b 가 실행될 때
# 함수 매개변수에 원본 값이 아니라 원본 주소를 넘기기 때문
a = [10, 100]
b = [5, 10]
print(mul(a,b), a, b)
```

    15 10 5
    [10, 100, 5, 10] [10, 100, 5, 10] [5, 10]
    


```python
c = (10, 100)
d = (5, 10)
# 불변 -> 원본 변경되지 않는다
print(mul(c,d), c,d)
```

    (10, 100, 5, 10) (10, 100) (5, 10)
    


```python
# 파이썬 불변형 예외
# str, bytes, frozenset, Tuple : 사본을 생성하지 않는다 -> 참조를 반환한다

tt1 = (1, 2, 3, 4, 5)
tt2 = tuple(tt1) # 생성자로 생성 -> 보통이면 id값 새로 반환
tt3 = tt1[:]

# 불변형이지만 파이썬이 효율/성능을 위해 이렇게 만들어짐
print(id(tt1), id(tt2), tt1 is tt2)
print(id(tt3), id(tt1),tt3 is tt1)
```

    2402081952680 2402081952680 True
    2402081952680 2402081952680 True
    


```python
tt4 = (10, 20, 30, 40, 50)
tt5 = (10, 20, 30, 40, 50)
ss1 = 'Apple'
ss2 = 'Apple'

# 튜플, str은 값이 같으면 같은 id 참조
print(tt4 is tt5, tt4 == tt5, id(tt4), id(tt5))
print(ss1 is ss2, ss1 == ss2, id(ss1), id(ss2))
```

    False True 2402112529992 2402112529704
    True True 2402112022640 2402112022640
    


```python

```
