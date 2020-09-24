# 반복형
파이썬 반복형 종류
- for, collections, text file, List, Dict, Set, Tuple, unpacking, *args
- 반복 가능한 이유 -> iter() 함수 호출


```python
t = 'ABCDEF'
w = iter(t)
print(type(w))
while True:
    try:
        print(next(w))
    except StopIteration:
        break
```

    <class 'str_iterator'>
    A
    B
    C
    D
    E
    F
    


```python
from collections import abc

# 반복형 확인
print('hasattr : ',hasattr(t, '__iter__'))
print(isinstance(t, abc.Iterable))

```

    hasattr :  True
    True
    


```python
# next 사용
class WordSplitIter:
    def __init__(self, text):
        self._idx = 0
        self._text = text.split(' ')
    
    def __next__(self):
        print('Called __next__')
        try:
            word = self._text[self._idx]
        except IndexError:
            raise StopIteration('Stop! Stop!')
        self._idx += 1
        return word
    
    def __iter__(self):
        print('Called __iter__')
        return self
    
    def __repr__(self):
        return 'WordSplit(%s)' % (self._text)

wi = WordSplitIter('Who says the nights are for sleeping')

print('repr : ', wi)
print(next(wi))
for _ in range(10):
    print(next(wi))
```

    repr :  WordSplit(['Who', 'says', 'the', 'nights', 'are', 'for', 'sleeping'])
    Called __next__
    Who
    Called __next__
    says
    Called __next__
    the
    Called __next__
    nights
    Called __next__
    are
    Called __next__
    for
    Called __next__
    sleeping
    Called __next__
    


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-6-20571a103635> in __next__(self)
          9         try:
    ---> 10             word = self._text[self._idx]
         11         except IndexError:
    

    IndexError: list index out of range

    
    During handling of the above exception, another exception occurred:
    

    StopIteration                             Traceback (most recent call last)

    <ipython-input-6-20571a103635> in <module>
         26 print(next(wi))
         27 for _ in range(10):
    ---> 28     print(next(wi))
    

    <ipython-input-6-20571a103635> in __next__(self)
         10             word = self._text[self._idx]
         11         except IndexError:
    ---> 12             raise StopIteration('Stop! Stop!')
         13         self._idx += 1
         14         return word
    

    StopIteration: Stop! Stop!


# Generator
- 위에서 구현한 내용을 Generator로 구현하면 더 간결
- \__next__ 구현안해도 괜찮다


# Generator패턴
1. 지능형 리스트, 딕셔너리, 집합 -> 데이터 셋이 증가 될 경우 메모리 사용량 증가 -> 제네레이터 완화
    - 한번 호출할 때마다 하나의 값만 리턴 - 매우 적은 메모리 양 필요로 한다
2. 단위 실행 가능한 코루틴(Coroutine) 구현에 아주 중요
3. 딕셔너리, 리스트 한 번 호출 할 때 마다 하나의 값만 리턴 -> 아주 작은 메모리 양을 필요로 함


```python
class WordSplitGenerator:
    def __init__(self, text):
        self._text = text.split(' ')
    
    def __iter__(self):
        for word in self._text:
           yield word # 제네레이터
        return
    
    def __repr__(self):
        return 'WordSplit(%s)' % (self._text)


wg = WordSplitGenerator('Who says the nights are for sleeping')

#iterable하게 만들기
wt = iter(wg)
for _ in range(7):
    print(next(wt))
```

    Who
    says
    the
    nights
    are
    for
    sleeping
    


```python
# Generator 예제
def generator_ex1():
    print('start')
    yield 'AAA'
    print('continue')
    yield 'BBB'
    print('end')
    
temp = iter(generator_ex1())
print(next(temp))
print()
print(next(temp))
print()
print(next(temp))
```

    start
    AAA
    
    continue
    BBB
    
    end
    


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-12-5adc3e413a94> in <module>
         12 print(next(temp))
         13 print()
    ---> 14 print(next(temp))
    

    StopIteration: 



```python
def generator_ex1():
    print('start')
    yield 'AAA'
    print('continue')
    yield 'BBB'
    print('end')

for v in generator_ex1():
    print(v)
```

    start
    AAA
    continue
    BBB
    end
    


```python
# generator 예제 2
temp2 = [x * 3 for x in generator_ex1()]
temp3 = (x * 3 for x in generator_ex1()) # 지능형 generator

print(temp2)
print()
print(temp3)
```

    start
    continue
    end
    ['AAAAAAAAA', 'BBBBBBBBB']
    
    <generator object <genexpr> at 0x000001A6FCB192C8>
    


```python
for i in temp3:
    print(i)
```

    start
    AAAAAAAAA
    continue
    BBBBBBBBB
    end
    

# generator - itertools


```python
import itertools

gen1 = itertools.count(1, 2.5) # 무한대의 수 만들어낸다
print(next(gen1))
print(next(gen1))
print(next(gen1))
```

    1
    3.5
    6.0
    


```python
# 조건
gen2 = itertools.takewhile(lambda n: n < 1000, itertools.count(1, 2.5))

for v in gen2:
    print(v, end = ' ')
```

    1 3.5 6.0 8.5 11.0 13.5 16.0 18.5 21.0 23.5 26.0 28.5 31.0 33.5 36.0 38.5 41.0 43.5 46.0 48.5 51.0 53.5 56.0 58.5 61.0 63.5 66.0 68.5 71.0 73.5 76.0 78.5 81.0 83.5 86.0 88.5 91.0 93.5 96.0 98.5 101.0 103.5 106.0 108.5 111.0 113.5 116.0 118.5 121.0 123.5 126.0 128.5 131.0 133.5 136.0 138.5 141.0 143.5 146.0 148.5 151.0 153.5 156.0 158.5 161.0 163.5 166.0 168.5 171.0 173.5 176.0 178.5 181.0 183.5 186.0 188.5 191.0 193.5 196.0 198.5 201.0 203.5 206.0 208.5 211.0 213.5 216.0 218.5 221.0 223.5 226.0 228.5 231.0 233.5 236.0 238.5 241.0 243.5 246.0 248.5 251.0 253.5 256.0 258.5 261.0 263.5 266.0 268.5 271.0 273.5 276.0 278.5 281.0 283.5 286.0 288.5 291.0 293.5 296.0 298.5 301.0 303.5 306.0 308.5 311.0 313.5 316.0 318.5 321.0 323.5 326.0 328.5 331.0 333.5 336.0 338.5 341.0 343.5 346.0 348.5 351.0 353.5 356.0 358.5 361.0 363.5 366.0 368.5 371.0 373.5 376.0 378.5 381.0 383.5 386.0 388.5 391.0 393.5 396.0 398.5 401.0 403.5 406.0 408.5 411.0 413.5 416.0 418.5 421.0 423.5 426.0 428.5 431.0 433.5 436.0 438.5 441.0 443.5 446.0 448.5 451.0 453.5 456.0 458.5 461.0 463.5 466.0 468.5 471.0 473.5 476.0 478.5 481.0 483.5 486.0 488.5 491.0 493.5 496.0 498.5 501.0 503.5 506.0 508.5 511.0 513.5 516.0 518.5 521.0 523.5 526.0 528.5 531.0 533.5 536.0 538.5 541.0 543.5 546.0 548.5 551.0 553.5 556.0 558.5 561.0 563.5 566.0 568.5 571.0 573.5 576.0 578.5 581.0 583.5 586.0 588.5 591.0 593.5 596.0 598.5 601.0 603.5 606.0 608.5 611.0 613.5 616.0 618.5 621.0 623.5 626.0 628.5 631.0 633.5 636.0 638.5 641.0 643.5 646.0 648.5 651.0 653.5 656.0 658.5 661.0 663.5 666.0 668.5 671.0 673.5 676.0 678.5 681.0 683.5 686.0 688.5 691.0 693.5 696.0 698.5 701.0 703.5 706.0 708.5 711.0 713.5 716.0 718.5 721.0 723.5 726.0 728.5 731.0 733.5 736.0 738.5 741.0 743.5 746.0 748.5 751.0 753.5 756.0 758.5 761.0 763.5 766.0 768.5 771.0 773.5 776.0 778.5 781.0 783.5 786.0 788.5 791.0 793.5 796.0 798.5 801.0 803.5 806.0 808.5 811.0 813.5 816.0 818.5 821.0 823.5 826.0 828.5 831.0 833.5 836.0 838.5 841.0 843.5 846.0 848.5 851.0 853.5 856.0 858.5 861.0 863.5 866.0 868.5 871.0 873.5 876.0 878.5 881.0 883.5 886.0 888.5 891.0 893.5 896.0 898.5 901.0 903.5 906.0 908.5 911.0 913.5 916.0 918.5 921.0 923.5 926.0 928.5 931.0 933.5 936.0 938.5 941.0 943.5 946.0 948.5 951.0 953.5 956.0 958.5 961.0 963.5 966.0 968.5 971.0 973.5 976.0 978.5 981.0 983.5 986.0 988.5 991.0 993.5 996.0 998.5 


```python
# 필터 반대
gen3 = itertools.filterfalse(lambda n : n < 3, [1,2,3,4,5])

for v in gen3:
    print(v)
```

    3
    4
    5
    


```python
# 누적 합계
gen4 = itertools.accumulate([x for x in range(1, 101)])

for v in gen4:
    print(v, end = ' ')
```

    1 3 6 10 15 21 28 36 45 55 66 78 91 105 120 136 153 171 190 210 231 253 276 300 325 351 378 406 435 465 496 528 561 595 630 666 703 741 780 820 861 903 946 990 1035 1081 1128 1176 1225 1275 1326 1378 1431 1485 1540 1596 1653 1711 1770 1830 1891 1953 2016 2080 2145 2211 2278 2346 2415 2485 2556 2628 2701 2775 2850 2926 3003 3081 3160 3240 3321 3403 3486 3570 3655 3741 3828 3916 4005 4095 4186 4278 4371 4465 4560 4656 4753 4851 4950 5050 


```python
# 연결1
gen5 = itertools.chain('ABCDE', range(1, 11, 2))
print(list(gen5))
```

    ['A', 'B', 'C', 'D', 'E', 1, 3, 5, 7, 9]
    


```python
# 연결2
gen6 = itertools.chain(enumerate('ABCDE'))
print(list(gen6))
```

    [(0, 'A'), (1, 'B'), (2, 'C'), (3, 'D'), (4, 'E')]
    


```python
# 개별
gen7 = itertools.product('ABCDE')
print(list(gen7))
```

    [('A',), ('B',), ('C',), ('D',), ('E',)]
    


```python
# 연산(경우의 수)
gen8 = itertools.product('ABCDE', repeat=2)
print(list(gen8))
```

    [('A', 'A'), ('A', 'B'), ('A', 'C'), ('A', 'D'), ('A', 'E'), ('B', 'A'), ('B', 'B'), ('B', 'C'), ('B', 'D'), ('B', 'E'), ('C', 'A'), ('C', 'B'), ('C', 'C'), ('C', 'D'), ('C', 'E'), ('D', 'A'), ('D', 'B'), ('D', 'C'), ('D', 'D'), ('D', 'E'), ('E', 'A'), ('E', 'B'), ('E', 'C'), ('E', 'D'), ('E', 'E')]
    


```python
# 그룹화
gen9 = itertools.groupby('AAABBCCCCDDEEE')
#print(list(gen9))
for chr, group in gen9:
    print(chr, ':', list(group))
```

    A : ['A', 'A', 'A']
    B : ['B', 'B']
    C : ['C', 'C', 'C', 'C']
    D : ['D', 'D']
    E : ['E', 'E', 'E']
    


```python

```
