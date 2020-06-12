## Intro trick

```python
import sys

a = [1,2]
b = [a,a]

sys.getsizeof(a) #72
sys.getsizeof(b) #72
```

## 1. Empty not empty

```python
import sys

sys.getsizeof("")       # 49
sys.getsizeof([])       # 56
sys.getsizeof(())       # 40
sys.getsizeof(set())    # 216
sys.getsizeof(dict())   # 232

sys.getsizeof(1)        # 28
sys.getsizeof(True)     # 28
```

## 2. Memory Expansion

```python
import sys
letters = "abcdefghijklmnopqrstuvwxyz"

a = []
for i in letters:
  a.append(i)
  print(f'{len(a)}, sys.getsizeof(a) = {sys.getsizeof(a)}')
  
b = set()
for j in letters:
  b.add(j)
  print(f'{len(b)}, sys.getsizeof(b) = {sys.getsizeof(b)}')
  
c = dict()
for k in letters:
  c[k]=k
  print(f'{len(c)}, sys.getsizeof(c) = {sys.getsizeof(c)}')
```

## 3. No memory Release

```python
import sys

a = [1,2,3,4]
sys.getsizeof(a)  # 88

a.append(5)
sys.getsizeof(a)  # 120

a.pop()
sys.getsizeof(a)  # 120
```

## 4. No empty dictionary

```python
import sys

a = [1,2,3]
b = {1,2,3}
c = {'a':1, 'b':2, 'c':3}

sys.getsizeof(a)  # 80
sys.getsizeof(b)  # 216
sys.getsizeof(c)  # 232

a.clear()
b.clear()
c.clear()
```
```python
sys.getsizeof(a)  # 56
sys.getsizeof(b)  # 216
sys.getsizeof(c)  # 64
```
