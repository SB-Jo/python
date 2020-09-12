# 변수

- 변수는 식별자이며, 상태를 관리한다
- 변수는 값을 저장한다.
- 파이썬은 이름공간(네임스페이스)에서 모듈, 클래스, 함수, 객체, 변수를 관리한다
 - 네임스페이스는 dict자료구조로 키와 값을 가진다
- 즉, 변수를 호출한다는 것은 네임스페이스에서 검색한다는 것이다

## 문법 규칙에 사용하는 예약어


```python
# keyword -> 예약어 관리 모듈
import keyword
print(keyword.kwlist)
```

    ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
    


```python
type(keyword.kwlist)
```




    list




```python
# 예약어는 변수로 사용할 수 없다
if = 100
```


      File "<ipython-input-3-8c2ff146cd4f>", line 2
        if = 100
           ^
    SyntaxError: invalid syntax
    


## 변수 이름을 짓는 문자열 알아보기


```python
# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수
import string
print(dir(string))
```

    ['Formatter', 'Template', '_ChainMap', '_TemplateMetaclass', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_re', '_string', 'ascii_letters', 'ascii_lowercase', 'ascii_uppercase', 'capwords', 'digits', 'hexdigits', 'octdigits', 'printable', 'punctuation', 'whitespace']
    


```python
# string 주요 변수들
string.ascii_letters
```




    'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'




```python
string.digits
```




    '0123456789'




```python
string.punctuation
```




    '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'




```python
type(string.punctuation)
```




    str




```python
string.whitespace
```




    ' \t\n\r\x0b\x0c'



## 변수 이름 정의 규칙
- 변수 권장 명명 규칙
 - 함수, 객체, 변수 이름 첫 글자는 소문자
 - 클래스 이름 첫 글자는 대문자
 - 이름 두 단어 이상 쓸 때는 두번째 단어 첫 글자 대문자
 - 클래스나 객체 내 보호 속성 정의할 때 첫 글자 밑줄(_)로 시작
 - 예약어와 같은 이름의 변수 이름을 사용하려면 예약어 뒤에 밑줄
 - 클래스나 객체의 비공개 속성은 외부에서 직접 접근할 수 없게 이름 변경하는 구조인 맹글링처리방식
  - 이름 앞에 double underscore(\__) 붙이면 된다
 - 파이썬 내부에서만 사용되는 스폐셜 속성이나 메소드는 이름 양쪽에 \__ 붙여 사용한다


```python
var = 100
_var = 100
__var = 100
var, _var, __var
```




    (100, 100, 100)




```python
var_ = 300
var__ =  400
var_, var__
```




    (300, 400)




```python
var is _var
```




    True




```python
var is __var
```




    True




```python
# 변수 이름 첫 글자에 숫자를 쓸 수 없다
9_var = 100
```


      File "<ipython-input-17-7a7a0f044bad>", line 2
        9_var = 100
         ^
    SyntaxError: invalid token
    


## 내장 이름공간과 전역 이름공간


```python
# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다
__builtins__
```




    <module 'builtins' (built-in)>




```python
__builtins__.globals
```




    <function globals()>




```python
# globals() : 전역이름공간을 조회해서 딕셔너리로 반환해주는 내장함수
# jupyter notebook은 하나의 파일이 하나의 모듈이기 때문에
# 모듈 내 정의된 모든 변수를 관리하는 전역 이름공간(global namespace)가 불러와진다
__builtins__.globals()
```




    {'__name__': '__main__',
     '__doc__': 'Automatically created module for IPython interactive environment',
     '__package__': None,
     '__loader__': None,
     '__spec__': None,
     '__builtin__': <module 'builtins' (built-in)>,
     '__builtins__': <module 'builtins' (built-in)>,
     '_ih': ['',
      '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)',
      'type(keyword.kwlist)',
      '# 예약어는 변수로 사용할 수 없다\nif = 100',
      '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)',
      '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))',
      '# string 주요 변수들\nstring.ascii_letters',
      'string.digits',
      'string.punctuation',
      'string.whitespace',
      'type(string.punctuation)',
      'var = 100, _var = 100, __var = 100\nvar, _var, __var',
      'var = 100\n_var = 100\n__var = 100\nvar, _var, __var',
      'var_ = 300\nvar__ 400\nvar_, var__',
      'var_ = 300\nvar__ =  400\nvar_, var__',
      'var is _var',
      'var is __var',
      '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100',
      '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__',
      '__builtins__.globals()'],
     '_oh': {2: list,
      4: ['Formatter',
       'Template',
       '_ChainMap',
       '_TemplateMetaclass',
       '__all__',
       '__builtins__',
       '__cached__',
       '__doc__',
       '__file__',
       '__loader__',
       '__name__',
       '__package__',
       '__spec__',
       '_re',
       '_string',
       'ascii_letters',
       'ascii_lowercase',
       'ascii_uppercase',
       'capwords',
       'digits',
       'hexdigits',
       'octdigits',
       'printable',
       'punctuation',
       'whitespace'],
      6: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ',
      7: '0123456789',
      8: '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~',
      9: ' \t\n\r\x0b\x0c',
      10: str,
      12: (100, 100, 100),
      14: (300, 400),
      15: True,
      16: True,
      18: <module 'builtins' (built-in)>},
     '_dh': ['C:\\Users\\User\\Downloads\\python\\Untitled Folder'],
     'In': ['',
      '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)',
      'type(keyword.kwlist)',
      '# 예약어는 변수로 사용할 수 없다\nif = 100',
      '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)',
      '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))',
      '# string 주요 변수들\nstring.ascii_letters',
      'string.digits',
      'string.punctuation',
      'string.whitespace',
      'type(string.punctuation)',
      'var = 100, _var = 100, __var = 100\nvar, _var, __var',
      'var = 100\n_var = 100\n__var = 100\nvar, _var, __var',
      'var_ = 300\nvar__ 400\nvar_, var__',
      'var_ = 300\nvar__ =  400\nvar_, var__',
      'var is _var',
      'var is __var',
      '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100',
      '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__',
      '__builtins__.globals()'],
     'Out': {2: list,
      4: ['Formatter',
       'Template',
       '_ChainMap',
       '_TemplateMetaclass',
       '__all__',
       '__builtins__',
       '__cached__',
       '__doc__',
       '__file__',
       '__loader__',
       '__name__',
       '__package__',
       '__spec__',
       '_re',
       '_string',
       'ascii_letters',
       'ascii_lowercase',
       'ascii_uppercase',
       'capwords',
       'digits',
       'hexdigits',
       'octdigits',
       'printable',
       'punctuation',
       'whitespace'],
      6: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ',
      7: '0123456789',
      8: '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~',
      9: ' \t\n\r\x0b\x0c',
      10: str,
      12: (100, 100, 100),
      14: (300, 400),
      15: True,
      16: True,
      18: <module 'builtins' (built-in)>},
     'get_ipython': <bound method InteractiveShell.get_ipython of <ipykernel.zmqshell.ZMQInteractiveShell object at 0x000002679B8735C8>>,
     'exit': <IPython.core.autocall.ZMQExitAutocall at 0x2679b9a22c8>,
     'quit': <IPython.core.autocall.ZMQExitAutocall at 0x2679b9a22c8>,
     '_': <module 'builtins' (built-in)>,
     '__': True,
     '___': True,
     '_i': '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__',
     '_ii': '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100',
     '_iii': 'var is __var',
     '_i1': '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)',
     'keyword': <module 'keyword' from 'c:\\users\\user\\anaconda3\\envs\\tf\\lib\\keyword.py'>,
     '_i2': 'type(keyword.kwlist)',
     '_2': list,
     '_i3': '# 예약어는 변수로 사용할 수 없다\nif = 100',
     '_i4': '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)',
     'string': <module 'string' from 'c:\\users\\user\\anaconda3\\envs\\tf\\lib\\string.py'>,
     '_4': ['Formatter',
      'Template',
      '_ChainMap',
      '_TemplateMetaclass',
      '__all__',
      '__builtins__',
      '__cached__',
      '__doc__',
      '__file__',
      '__loader__',
      '__name__',
      '__package__',
      '__spec__',
      '_re',
      '_string',
      'ascii_letters',
      'ascii_lowercase',
      'ascii_uppercase',
      'capwords',
      'digits',
      'hexdigits',
      'octdigits',
      'printable',
      'punctuation',
      'whitespace'],
     '_i5': '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))',
     '_i6': '# string 주요 변수들\nstring.ascii_letters',
     '_6': 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ',
     '_i7': 'string.digits',
     '_7': '0123456789',
     '_i8': 'string.punctuation',
     '_8': '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~',
     '_i9': 'string.whitespace',
     '_9': ' \t\n\r\x0b\x0c',
     '_i10': 'type(string.punctuation)',
     '_10': str,
     '_i11': 'var = 100, _var = 100, __var = 100\nvar, _var, __var',
     '_i12': 'var = 100\n_var = 100\n__var = 100\nvar, _var, __var',
     'var': 100,
     '_var': 100,
     '__var': 100,
     '_12': (100, 100, 100),
     '_i13': 'var_ = 300\nvar__ 400\nvar_, var__',
     '_i14': 'var_ = 300\nvar__ =  400\nvar_, var__',
     'var_': 300,
     'var__': 400,
     '_14': (300, 400),
     '_i15': 'var is _var',
     '_15': True,
     '_i16': 'var is __var',
     '_16': True,
     '_i17': '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100',
     '_i18': '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__',
     '_18': <module 'builtins' (built-in)>,
     '_i19': '__builtins__.globals()'}




```python
i = 100
globals()['i']
```




    100




```python
# vars() : 전역 이름공간에 저장된 변수를 조회하는 내장함수
__builtins__.vars
```




    <function vars>




```python
print(__builtins__.vars())
```

    {'__name__': '__main__', '__doc__': 'Automatically created module for IPython interactive environment', '__package__': None, '__loader__': None, '__spec__': None, '__builtin__': <module 'builtins' (built-in)>, '__builtins__': <module 'builtins' (built-in)>, '_ih': ['', '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)', 'type(keyword.kwlist)', '# 예약어는 변수로 사용할 수 없다\nif = 100', '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)', '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))', '# string 주요 변수들\nstring.ascii_letters', 'string.digits', 'string.punctuation', 'string.whitespace', 'type(string.punctuation)', 'var = 100, _var = 100, __var = 100\nvar, _var, __var', 'var = 100\n_var = 100\n__var = 100\nvar, _var, __var', 'var_ = 300\nvar__ 400\nvar_, var__', 'var_ = 300\nvar__ =  400\nvar_, var__', 'var is _var', 'var is __var', '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100', '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__', '__builtins__.globals()', '__builtins__.globals', "ㅑ i = 100\nglobals()['i']", "i = 100\nglobals()['i']", '# vars() : 전역 이름공간에 저장된 변수를 조회하는 내장함수\n__builtins__.vars', '__builtins__.vars()', 'print(__builtins__.vars())'], '_oh': {2: <class 'list'>, 4: ['Formatter', 'Template', '_ChainMap', '_TemplateMetaclass', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_re', '_string', 'ascii_letters', 'ascii_lowercase', 'ascii_uppercase', 'capwords', 'digits', 'hexdigits', 'octdigits', 'printable', 'punctuation', 'whitespace'], 6: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ', 7: '0123456789', 8: '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~', 9: ' \t\n\r\x0b\x0c', 10: <class 'str'>, 12: (100, 100, 100), 14: (300, 400), 15: True, 16: True, 18: <module 'builtins' (built-in)>, 19: {...}, 20: <built-in function globals>, 22: 100, 23: <built-in function vars>, 24: {...}}, '_dh': ['C:\\Users\\User\\Downloads\\python\\Untitled Folder'], 'In': ['', '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)', 'type(keyword.kwlist)', '# 예약어는 변수로 사용할 수 없다\nif = 100', '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)', '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))', '# string 주요 변수들\nstring.ascii_letters', 'string.digits', 'string.punctuation', 'string.whitespace', 'type(string.punctuation)', 'var = 100, _var = 100, __var = 100\nvar, _var, __var', 'var = 100\n_var = 100\n__var = 100\nvar, _var, __var', 'var_ = 300\nvar__ 400\nvar_, var__', 'var_ = 300\nvar__ =  400\nvar_, var__', 'var is _var', 'var is __var', '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100', '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__', '__builtins__.globals()', '__builtins__.globals', "ㅑ i = 100\nglobals()['i']", "i = 100\nglobals()['i']", '# vars() : 전역 이름공간에 저장된 변수를 조회하는 내장함수\n__builtins__.vars', '__builtins__.vars()', 'print(__builtins__.vars())'], 'Out': {2: <class 'list'>, 4: ['Formatter', 'Template', '_ChainMap', '_TemplateMetaclass', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_re', '_string', 'ascii_letters', 'ascii_lowercase', 'ascii_uppercase', 'capwords', 'digits', 'hexdigits', 'octdigits', 'printable', 'punctuation', 'whitespace'], 6: 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ', 7: '0123456789', 8: '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~', 9: ' \t\n\r\x0b\x0c', 10: <class 'str'>, 12: (100, 100, 100), 14: (300, 400), 15: True, 16: True, 18: <module 'builtins' (built-in)>, 19: {...}, 20: <built-in function globals>, 22: 100, 23: <built-in function vars>, 24: {...}}, 'get_ipython': <bound method InteractiveShell.get_ipython of <ipykernel.zmqshell.ZMQInteractiveShell object at 0x000002679B8735C8>>, 'exit': <IPython.core.autocall.ZMQExitAutocall object at 0x000002679B9A22C8>, 'quit': <IPython.core.autocall.ZMQExitAutocall object at 0x000002679B9A22C8>, '_': {...}, '__': <built-in function vars>, '___': 100, '_i': '__builtins__.vars()', '_ii': '# vars() : 전역 이름공간에 저장된 변수를 조회하는 내장함수\n__builtins__.vars', '_iii': "i = 100\nglobals()['i']", '_i1': '# keyword -> 예약어 관리 모듈\nimport keyword\nprint(keyword.kwlist)', 'keyword': <module 'keyword' from 'c:\\users\\user\\anaconda3\\envs\\tf\\lib\\keyword.py'>, '_i2': 'type(keyword.kwlist)', '_2': <class 'list'>, '_i3': '# 예약어는 변수로 사용할 수 없다\nif = 100', '_i4': '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\ndir(string)', 'string': <module 'string' from 'c:\\users\\user\\anaconda3\\envs\\tf\\lib\\string.py'>, '_4': ['Formatter', 'Template', '_ChainMap', '_TemplateMetaclass', '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', '_re', '_string', 'ascii_letters', 'ascii_lowercase', 'ascii_uppercase', 'capwords', 'digits', 'hexdigits', 'octdigits', 'printable', 'punctuation', 'whitespace'], '_i5': '# dir - 해당 객체가 어떤 변수와 메소드를 가지고 있는지 나타내는 내장함수\nimport string\nprint(dir(string))', '_i6': '# string 주요 변수들\nstring.ascii_letters', '_6': 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ', '_i7': 'string.digits', '_7': '0123456789', '_i8': 'string.punctuation', '_8': '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~', '_i9': 'string.whitespace', '_9': ' \t\n\r\x0b\x0c', '_i10': 'type(string.punctuation)', '_10': <class 'str'>, '_i11': 'var = 100, _var = 100, __var = 100\nvar, _var, __var', '_i12': 'var = 100\n_var = 100\n__var = 100\nvar, _var, __var', 'var': 100, '_var': 100, '__var': 100, '_12': (100, 100, 100), '_i13': 'var_ = 300\nvar__ 400\nvar_, var__', '_i14': 'var_ = 300\nvar__ =  400\nvar_, var__', 'var_': 300, 'var__': 400, '_14': (300, 400), '_i15': 'var is _var', '_15': True, '_i16': 'var is __var', '_16': True, '_i17': '# 변수 이름 첫 글자에 숫자를 쓸 수 없다\n9_var = 100', '_i18': '# 파이썬에서 기본적으로 제공하는 함수와 클래스는 __builtins__ 에서 관리한다\n__builtins__', '_18': <module 'builtins' (built-in)>, '_i19': '__builtins__.globals()', '_19': {...}, '_i20': '__builtins__.globals', '_20': <built-in function globals>, '_i21': "ㅑ i = 100\nglobals()['i']", '_i22': "i = 100\nglobals()['i']", 'i': 100, '_22': 100, '_i23': '# vars() : 전역 이름공간에 저장된 변수를 조회하는 내장함수\n__builtins__.vars', '_23': <built-in function vars>, '_i24': '__builtins__.vars()', '_24': {...}, '_i25': 'print(__builtins__.vars())'}
    


```python
def add(x,y):
    return x+y
```


```python
globals()['add']
```




    <function __main__.add(x, y)>




```python
# 전역 이름공간 공유하고 있다
add.__globals__['i']
```




    100




```python
# 함수가 연동하고 있는 전역공간과 전역공간이 같음
add.__globals__ is globals()
```




    True




```python

```
