```python
str_01 = "Niceman"
print('x' in str_01)
print('N' in str_01)
```

    False
    True
    


```python
print(str_01 * 2)
```

    NicemanNiceman
    


```python
str_02 = 'man'
print(str_01 + str_02)
print(str_01.join(["I'm ", "!"]))
print(str_01.replace("Nice", "good"), str_01)
```

    Nicemanman
    I'm Niceman!
    goodman Niceman
    


```python
print(sorted(str_01))
print(reversed(str_02))
print(list(reversed(str_02)))
```

    ['N', 'a', 'c', 'e', 'i', 'm', 'n']
    <reversed object at 0x000002821ED64188>
    ['n', 'a', 'm']
    


```python
#[start, end, step]
print(str_01[1:4:2])
print(str_01[::-1])
print(str_01[::2])
```

    ie
    nameciN
    Ncmn
    


```python
del str_01
print(str_01)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-17-e61af3bb9940> in <module>
          1 del str_01
    ----> 2 print(str_01)
    

    NameError: name 'str_01' is not defined

