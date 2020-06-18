# Ways to Merge lists
## 1. Addition
```Python
list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

list01 + list02 + list03
Output : [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
## 2. Itertools
```Python
from itertools import chain

list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

list(chain(list01, list02, list03))
Output : [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
## 3. * to unpack
```Python
list01 = [1,2,3]
list02 = [4,5,6]

[*list01, *list02]
Output: [1, 2, 3, 4, 5, 6]
```
## 4. Extend
```Python
list01 = [1,2,3]
list02 = [4,5,6]

list01.extend(list02)
list01

Output : [1, 2, 3, 4, 5, 6]
```
## 5. List comprehension
```Python
list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

[x for l in (list01, list02, list03) for x in l]
Output : [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 6. Heapq
```Python
list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

from heapq import merge
list(merge(list01, list02, list03))

Output : [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 7. Magic methods
```Python
list01 = [1,2,3]
list02 = [4,5,6]
 
list01 + list02
Output: [1, 2, 3, 4, 5, 6]
 
list01.__add__(list02)
Output: [1, 2, 3, 4, 5, 6]
```
```python
list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

from functools import reduce
reduce(list.__add__, (list01, list02, list03))

Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```
## 8. yield from
```Python
list01 = [1,2,3]
list02 = [4,5,6]
list03 = [7,8,9]

def merge(*lists):
    for l in lists:
        yield from l
        
list(merge(list01, list02, list03))
Output:[1, 2, 3, 4, 5, 6, 7, 8, 9]
```
