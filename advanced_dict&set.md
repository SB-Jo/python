# DICT
- 중복 허용 X -> 내부적으로 해시를 이용하고 있다
- 적은 리소스로 많은 데이터를 효율적으로 관리 가능하다


```python
# HASH 값 확인
t1 = (10, 20, (30, 40, 50))
t2 = (10, 20, [30, 40, 50])
print(hash(t1))
```

    5737367089334957572
    


```python
# list는 hash값 갖지 않는다
print(hash(t2))
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-3-5041aa91eb08> in <module>
    ----> 1 print(hash(t2))
    

    TypeError: unhashable type: 'list'



```python
# Dict SetDefault 예제
source = (('k1', 'val1'),
            ('k1', 'val2'),
            ('k2', 'val3'),
            ('k2', 'val4'),
            ('k2', 'val5'))

new_dict1 = {}
new_dict2 = {}
```


```python
# No Use Setdefault
for k, v in source:
    if k in new_dict1:
        new_dict1[k].append(v)
    else:
        new_dict1[k] = [v]

new_dict1
```




    {'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}




```python
# Use Setdefault
for k, v in source:
    new_dict2.setdefault(k, []).append(v)

new_dict2
```




    {'k1': ['val1', 'val2'], 'k2': ['val3', 'val4', 'val5']}




```python
# 사용자 정의 dict 상속(UserDict)
# dict는 파이썬 내부적으로 아래와 같이 이루어져있다
class UserDict(dict):
    def __missing__(self, key):
        print('Called : __missing__')
        if isinstance(key, str):
            raise KeyError(key)
        return self[str(key)]
    
    def get(self, key, default=None):
        print('Called : __getitem__')
        try:
            return self[key]
        except KeyError:
            return default
    
    def __contains__(self, key):
        print('Called : __contains__')
        return key in self.keys() or str(key) in self.keys()
```


```python
user_dict1 = UserDict(one=1, two=2)
user_dict2 = UserDict({'one': 1, 'two': 2})
user_dict3 = UserDict([('one',1),('two',2)])
```


```python
print(user_dict1, user_dict2, user_dict3)
```

    {'one': 1, 'two': 2} {'one': 1, 'two': 2} {'one': 1, 'two': 2}
    


```python
user_dict2.get('two')
```

    Called : __getitem__
    




    2




```python
user_dict2.get('aaa')
```

    Called : __getitem__
    Called : __missing__
    


```python
'one' in user_dict3
```

    Called : __contains__
    




    True




```python
user_dict3['three']
```

    Called : __missing__
    


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-14-36e7fbde3ec8> in <module>
    ----> 1 user_dict3['three']
    

    <ipython-input-8-5739a39d9a07> in __missing__(self, key)
          4         print('Called : __missing__')
          5         if isinstance(key, str):
    ----> 6             raise KeyError(key)
          7         return self[str(key)]
          8 
    

    KeyError: 'three'


# MappingProxyType
- immutable Dict가 필요할 때 MappingProxyType 사용


```python
d = {'key1': 'TEST1'}
```


```python
from types import MappingProxyType
d_frozen = MappingProxyType(d)
```


```python
d, id(d)
```




    ({'key1': 'TEST1'}, 2157595493624)




```python
d_frozen, id(d_frozen)
```




    (mappingproxy({'key1': 'TEST1'}), 2157637509032)




```python
# 내부 구조의 key와 value는 같기 때문에 'd == d_frozen'은 True가 나온다
d is d_frozen, d == d_frozen
```




    (False, True)




```python
# immutable
# 원본은 수정 가능하다
d_frozen['key2'] = 'TEST2'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-20-dd34060c8346> in <module>
    ----> 1 d_frozen['key2'] = 'TEST2'
    

    TypeError: 'mappingproxy' object does not support item assignment


# SET
- set은 중복 데이터가 많은 대용량 데이터에서는 속도가 느리다. 정제된 데이터에서 효율적
 - 자체적으로 중복 데이터 없애기 때문


```python
# set 선언하기
s1 = {'Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'}
# 리스트 형태로 넣어도 된다
s2 = set(['Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'])
s3 = {3}
s4 = set() # Not {} -> {}는 Dict
# frozenset -> 추가, 수정 불가
s5 = frozenset({'Apple', 'Orange', 'Apple', 'Orange', 'Kiwi'})
```


```python
print(s1, type(s1))
print(s2, type(s2))
print(s3, type(s3))
print(s4, type(s4))
print(s5, type(s5))
```

    {'Apple', 'Orange', 'Kiwi'} <class 'set'>
    {'Apple', 'Orange', 'Kiwi'} <class 'set'>
    {3} <class 'set'>
    set() <class 'set'>
    frozenset({'Apple', 'Orange', 'Kiwi'}) <class 'frozenset'>
    


```python
#set 선언 최적화
from dis import dis
print(dis('{10}'))
```

      1           0 LOAD_CONST               0 (10)
                  2 BUILD_SET                1
                  4 RETURN_VALUE
    None
    


```python
# set에 list 넣으면 5단계 소요 위에 언급된 방법이 더 효율적이다
print(dis('set([10])'))
```

      1           0 LOAD_NAME                0 (set)
                  2 LOAD_CONST               0 (10)
                  4 BUILD_LIST               1
                  6 CALL_FUNCTION            1
                  8 RETURN_VALUE
    None
    

## 지능형 집합(comprehending set)


```python
from unicodedata import name
# 0~256의 유니코드데이터 보여준다
{name(chr(i), '')for i in range(0, 256)}
```




    {'',
     'ACUTE ACCENT',
     'AMPERSAND',
     'APOSTROPHE',
     'ASTERISK',
     'BROKEN BAR',
     'CEDILLA',
     'CENT SIGN',
     'CIRCUMFLEX ACCENT',
     'COLON',
     'COMMA',
     'COMMERCIAL AT',
     'COPYRIGHT SIGN',
     'CURRENCY SIGN',
     'DEGREE SIGN',
     'DIAERESIS',
     'DIGIT EIGHT',
     'DIGIT FIVE',
     'DIGIT FOUR',
     'DIGIT NINE',
     'DIGIT ONE',
     'DIGIT SEVEN',
     'DIGIT SIX',
     'DIGIT THREE',
     'DIGIT TWO',
     'DIGIT ZERO',
     'DIVISION SIGN',
     'DOLLAR SIGN',
     'EQUALS SIGN',
     'EXCLAMATION MARK',
     'FEMININE ORDINAL INDICATOR',
     'FULL STOP',
     'GRAVE ACCENT',
     'GREATER-THAN SIGN',
     'HYPHEN-MINUS',
     'INVERTED EXCLAMATION MARK',
     'INVERTED QUESTION MARK',
     'LATIN CAPITAL LETTER A',
     'LATIN CAPITAL LETTER A WITH ACUTE',
     'LATIN CAPITAL LETTER A WITH CIRCUMFLEX',
     'LATIN CAPITAL LETTER A WITH DIAERESIS',
     'LATIN CAPITAL LETTER A WITH GRAVE',
     'LATIN CAPITAL LETTER A WITH RING ABOVE',
     'LATIN CAPITAL LETTER A WITH TILDE',
     'LATIN CAPITAL LETTER AE',
     'LATIN CAPITAL LETTER B',
     'LATIN CAPITAL LETTER C',
     'LATIN CAPITAL LETTER C WITH CEDILLA',
     'LATIN CAPITAL LETTER D',
     'LATIN CAPITAL LETTER E',
     'LATIN CAPITAL LETTER E WITH ACUTE',
     'LATIN CAPITAL LETTER E WITH CIRCUMFLEX',
     'LATIN CAPITAL LETTER E WITH DIAERESIS',
     'LATIN CAPITAL LETTER E WITH GRAVE',
     'LATIN CAPITAL LETTER ETH',
     'LATIN CAPITAL LETTER F',
     'LATIN CAPITAL LETTER G',
     'LATIN CAPITAL LETTER H',
     'LATIN CAPITAL LETTER I',
     'LATIN CAPITAL LETTER I WITH ACUTE',
     'LATIN CAPITAL LETTER I WITH CIRCUMFLEX',
     'LATIN CAPITAL LETTER I WITH DIAERESIS',
     'LATIN CAPITAL LETTER I WITH GRAVE',
     'LATIN CAPITAL LETTER J',
     'LATIN CAPITAL LETTER K',
     'LATIN CAPITAL LETTER L',
     'LATIN CAPITAL LETTER M',
     'LATIN CAPITAL LETTER N',
     'LATIN CAPITAL LETTER N WITH TILDE',
     'LATIN CAPITAL LETTER O',
     'LATIN CAPITAL LETTER O WITH ACUTE',
     'LATIN CAPITAL LETTER O WITH CIRCUMFLEX',
     'LATIN CAPITAL LETTER O WITH DIAERESIS',
     'LATIN CAPITAL LETTER O WITH GRAVE',
     'LATIN CAPITAL LETTER O WITH STROKE',
     'LATIN CAPITAL LETTER O WITH TILDE',
     'LATIN CAPITAL LETTER P',
     'LATIN CAPITAL LETTER Q',
     'LATIN CAPITAL LETTER R',
     'LATIN CAPITAL LETTER S',
     'LATIN CAPITAL LETTER T',
     'LATIN CAPITAL LETTER THORN',
     'LATIN CAPITAL LETTER U',
     'LATIN CAPITAL LETTER U WITH ACUTE',
     'LATIN CAPITAL LETTER U WITH CIRCUMFLEX',
     'LATIN CAPITAL LETTER U WITH DIAERESIS',
     'LATIN CAPITAL LETTER U WITH GRAVE',
     'LATIN CAPITAL LETTER V',
     'LATIN CAPITAL LETTER W',
     'LATIN CAPITAL LETTER X',
     'LATIN CAPITAL LETTER Y',
     'LATIN CAPITAL LETTER Y WITH ACUTE',
     'LATIN CAPITAL LETTER Z',
     'LATIN SMALL LETTER A',
     'LATIN SMALL LETTER A WITH ACUTE',
     'LATIN SMALL LETTER A WITH CIRCUMFLEX',
     'LATIN SMALL LETTER A WITH DIAERESIS',
     'LATIN SMALL LETTER A WITH GRAVE',
     'LATIN SMALL LETTER A WITH RING ABOVE',
     'LATIN SMALL LETTER A WITH TILDE',
     'LATIN SMALL LETTER AE',
     'LATIN SMALL LETTER B',
     'LATIN SMALL LETTER C',
     'LATIN SMALL LETTER C WITH CEDILLA',
     'LATIN SMALL LETTER D',
     'LATIN SMALL LETTER E',
     'LATIN SMALL LETTER E WITH ACUTE',
     'LATIN SMALL LETTER E WITH CIRCUMFLEX',
     'LATIN SMALL LETTER E WITH DIAERESIS',
     'LATIN SMALL LETTER E WITH GRAVE',
     'LATIN SMALL LETTER ETH',
     'LATIN SMALL LETTER F',
     'LATIN SMALL LETTER G',
     'LATIN SMALL LETTER H',
     'LATIN SMALL LETTER I',
     'LATIN SMALL LETTER I WITH ACUTE',
     'LATIN SMALL LETTER I WITH CIRCUMFLEX',
     'LATIN SMALL LETTER I WITH DIAERESIS',
     'LATIN SMALL LETTER I WITH GRAVE',
     'LATIN SMALL LETTER J',
     'LATIN SMALL LETTER K',
     'LATIN SMALL LETTER L',
     'LATIN SMALL LETTER M',
     'LATIN SMALL LETTER N',
     'LATIN SMALL LETTER N WITH TILDE',
     'LATIN SMALL LETTER O',
     'LATIN SMALL LETTER O WITH ACUTE',
     'LATIN SMALL LETTER O WITH CIRCUMFLEX',
     'LATIN SMALL LETTER O WITH DIAERESIS',
     'LATIN SMALL LETTER O WITH GRAVE',
     'LATIN SMALL LETTER O WITH STROKE',
     'LATIN SMALL LETTER O WITH TILDE',
     'LATIN SMALL LETTER P',
     'LATIN SMALL LETTER Q',
     'LATIN SMALL LETTER R',
     'LATIN SMALL LETTER S',
     'LATIN SMALL LETTER SHARP S',
     'LATIN SMALL LETTER T',
     'LATIN SMALL LETTER THORN',
     'LATIN SMALL LETTER U',
     'LATIN SMALL LETTER U WITH ACUTE',
     'LATIN SMALL LETTER U WITH CIRCUMFLEX',
     'LATIN SMALL LETTER U WITH DIAERESIS',
     'LATIN SMALL LETTER U WITH GRAVE',
     'LATIN SMALL LETTER V',
     'LATIN SMALL LETTER W',
     'LATIN SMALL LETTER X',
     'LATIN SMALL LETTER Y',
     'LATIN SMALL LETTER Y WITH ACUTE',
     'LATIN SMALL LETTER Y WITH DIAERESIS',
     'LATIN SMALL LETTER Z',
     'LEFT CURLY BRACKET',
     'LEFT PARENTHESIS',
     'LEFT SQUARE BRACKET',
     'LEFT-POINTING DOUBLE ANGLE QUOTATION MARK',
     'LESS-THAN SIGN',
     'LOW LINE',
     'MACRON',
     'MASCULINE ORDINAL INDICATOR',
     'MICRO SIGN',
     'MIDDLE DOT',
     'MULTIPLICATION SIGN',
     'NO-BREAK SPACE',
     'NOT SIGN',
     'NUMBER SIGN',
     'PERCENT SIGN',
     'PILCROW SIGN',
     'PLUS SIGN',
     'PLUS-MINUS SIGN',
     'POUND SIGN',
     'QUESTION MARK',
     'QUOTATION MARK',
     'REGISTERED SIGN',
     'REVERSE SOLIDUS',
     'RIGHT CURLY BRACKET',
     'RIGHT PARENTHESIS',
     'RIGHT SQUARE BRACKET',
     'RIGHT-POINTING DOUBLE ANGLE QUOTATION MARK',
     'SECTION SIGN',
     'SEMICOLON',
     'SOFT HYPHEN',
     'SOLIDUS',
     'SPACE',
     'SUPERSCRIPT ONE',
     'SUPERSCRIPT THREE',
     'SUPERSCRIPT TWO',
     'TILDE',
     'VERTICAL LINE',
     'VULGAR FRACTION ONE HALF',
     'VULGAR FRACTION ONE QUARTER',
     'VULGAR FRACTION THREE QUARTERS',
     'YEN SIGN'}




```python

```
