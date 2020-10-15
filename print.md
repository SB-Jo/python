```python
print('T', 'E', 'S', 'T', sep='-')
```

    T-E-S-T
    


```python
print('T', 'E', 'S', 'T', end='-')
```

    T E S T-


```python
# format 사용
print("%s's favorite number is %d"%('SB', int(45)))
```

    SB's favorite number is 45
    


```python
print('Test1: %5d, Price: %4.2f'%(776, 6534.123))
```

    Test1:   776, Price: 6534.12
    


```python
print('Test2 : {0:5d}, Price : {1:4.2f}'.format(776, 6534.123))
```

    Test2 :   776, Price : 6534.12
    


```python
print('{} and {}'.format('You', 'Me'))
print('{0} and {1} and {0}'.format('You', 'Me'))
print('{var1} and {var2}'.format(var1 = 'You', var2='me'))
```

    You and Me
    You and Me and You
    You and me
    


```python
import sys
# 입력 인코딩
print(sys.stdin.encoding)
```

    cp949
    


```python
# 출력 인코딩
print(sys.stdout.encoding)
```

    UTF-8
    
