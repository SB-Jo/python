# 유니코드 문자열 클래스와 바이트 문자열 클래스
- str : 유니코드 문자 사용
- bytes : 아스키 문자 사용


```python
s = str("문자열")
# 바이트 문자열은 앞에 b를 붙여 명시해야 한다
b = bytes(b"string")
print(s, b)
```

    문자열 b'string'
    


```python
# encode : 암호화
set(dir(str)) - set(dir(bytes))
```




    {'casefold',
     'encode',
     'format',
     'format_map',
     'isdecimal',
     'isidentifier',
     'isnumeric',
     'isprintable'}




```python
# decode : 복호화
set(dir(bytes)) - set(dir(str))
```




    {'decode', 'fromhex', 'hex'}




```python
bb = b'Hello'
# 바이트 문자열은 바이트 단위로 길이 계산
print(len(bb))

# hex() : 16진수값 확인. 16진수 표시인 x를 제외한 숫자 값만 표시
print(bb.hex())
```

    5
    48656c6c6f
    


```python
ss = 'Hello'
# str 문자열은 문자 개수로 길이 계산
print(len(ss))

# casefold() : 단어 첫 글자를 소문자로 변경
print(ss.casefold())
```

    5
    hello
    


```python
# encode: 유니코드 -> 바이트
ss_ = ss.encode()

# 영어는 한 문자가 한 바이트라 동일
print(ss_.hex())
print(len(ss_))
```

    48656c6c6f
    5
    


```python
# 한글은 한 문자가 3 바이트
sss = '안녕'
print(len(sss))
sss_ = ss.encode()
print(len(sss_))
```

    2
    5
    


```python
# bytearray : 변경할 수 있는 문자열 클래스
# 바이트 문자열을 바이트 배열로 변경 가능
a = bytearray(bytes(b'python'))
a
```




    bytearray(b'python')




```python
# 변경할 수 있는 객체인 경우 스폐셜 메소드 __setitem__ 이 구현되어 있다
a.__setitem__
```




    <method-wrapper '__setitem__' of bytearray object at 0x000001E193AFB4F0>




```python
# 바이트 문자가 어떤 글자인지 확인하려면 chr() 이용
# ord() : 문자를 바이트로
print(a[0])
print(chr(a[0]))
print(ord('p'))
```

    112
    p
    112
    


```python
a[0] = ord('c')
a
```




    bytearray(b'cython')



# 유니코드 문자열과 바이트 문자열 인코딩
- 유니코드 문자를 바이트 문자로 변환할 때 한글 인코딩은 기본으로 utf-8 사용
- utf-8은 한글 한 글자를 3바이트로 변환
- utf-16은 한글 한 글자를 2바이트로 변호나


```python
sss = '안녕하세요'
sss_1 = sss.encode(encoding='utf-16')
print(sss_1.hex())
# 10이 아닌 12인 이유 = fffe 때문.
# fffe = utf-16에서 바이트 순서 표시(BOM)
print(len(sss_1))
```

    fffe48c555b158d538c194c6
    12
    


```python
# 바이트 오더를 빅 엔디언으로 처리
# utf-16be = 두 바이트 한글로 처리
sss_2 = sss.encode(encoding = 'utf-16be')
print(sss_2.hex())
print(len(sss_2))
```

    c548b155d558c138c694
    10
    


```python
# 리틀 엔디엔(little endian)으로 지정-> 2바이트 코드가 추가되지 않는다
sss_3 = sss.encode(encoding='utf-16le')
print(sss_3.hex())
print(len(sss_3))
```

    48c555b158d538c194c6
    10
    


```python
# decode할 때 인코딩을 정확히 인자로 전달해야 한다.
print(sss_2.decode(encoding='utf-16be'))
# 그렇지 않으면 다른 언어로 변환
print(sss_2.decode(encoding='utf-16le'))
```

    안녕하세요
    䣅喱壕㣁铆
    


```python

```
