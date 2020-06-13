# Python Optimization Tricks

## 1. Avoid global variables
**Wrong way**
```python
import math
size = 10000
for x in range(size):
    for y in range(size):
        z = math.sqrt(x) + math.sqrt(y)
```
**Correct way**
```python
import math
def main():
    size = 10000
    for x in range(size):
        for y in range(size):
            z = math.sqrt(x) + math.sqrt(y)
main()
```
## 2.1 Avoid access to module and function properties
**Wrong way**
```python
import math
def computeSqrt(size: int):
    result = []
    for i in range(size):
        result.append(math.sqrt(i))
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()
```
**Correct Way**
```python
from math import sqrt
def computeSqrt(size: int):
    result = []
    for i in range(size):
        result.append(sqrt(i))
    return result
def main():
    size = 10000
    for _ in range(size):
        result = computeSqrt(size)
main()
```
## 2.2 Avoid intra-class attribute access
**Wrong way**
```python
import math
from typing import List
class DemoClass:
    def __init__(self, value: int):
        self._value = value

    def computeSqrt(self, size: int) -> List[float]:
        result = []
        append = result.append
        sqrt = math.sqrt
        for _ in range(size):
            append(sqrt(self._value))
        return result
def main():
    size = 10000
    for _ in range(size):
        demo_instance = DemoClass(size)
        result = demo_instance.computeSqrt(size)
main()
```
**Correct way**
```python
import math
from typing import List
class DemoClass:
    def __init__(self, value: int):
        self._value = value

    def computeSqrt(self, size: int) -> List[float]:
        result = []
        append = result.append
        sqrt = math.sqrt
        value = self._value
        for _ in range(size):
            append(sqrt(value))
        return result
def main():
    size = 10000
    for _ in range(size):
        demo_instance = DemoClass(size)
        demo_instance.computeSqrt(size)
main()
## 3. Avoid unnecessary abstractions
```
**Wrong way**
```python
class DemoClass:
    def __init__(self, value: int):
        self.value = value
@property
    def value(self) -> int:
        return self._value
@value.setter
    def value(self, x: int):
        self._value = x
def main():
    size = 1000000
    for i in range(size):
        demo_instance = DemoClass(size)
        value = demo_instance.value
        demo_instance.value = i
main()
```
**Correct way**
```python
class DemoClass:
    def __init__(self, value: int):
        self.value = value
def main():
    size = 1000000
    for i in range(size):
        demo_instance = DemoClass(size)
        value = demo_instance.value
        demo_instance.value = i
main()
```
## 4.1 Avoid meaningless data copying
**Wrong Way**
```python
def main():
    size = 10000
    for _ in range(size):
        value = range(size)
        value_list = [x for x in value]
        square_list = [x * x for x in value_list]
main()
```
**Correct Way**
```
def main():
    size = 10000
    for _ in range(size):
        value = range(size)
        square_list = [x * x for x in value]
main()
```
## 4.2 Intermediate variables are not used when exchanging values
**Wrong Way**
```python
def main():
    size = 1000000
    for _ in range(size):
        a = 3
        b = 5
        temp = a
        a = b
        b = temp
main()
```
**Correct Way**
```python
def main():
    size = 1000000
    for _ in range(size):
        a = 3
        b = 5
        a, b = b, a
main()
```
## 4.3 String concatenation join instead of '+'
**Wrong Way**
```python
import string
from typing import List
def concatString(string_list: List[str]) -> str:
    result = ''
    for str_i in string_list:
        result += str_i
    return result
def main():
    string_list = list(string.ascii_letters * 100)
    for _ in range(10000):
        result = concatString(string_list)
main()
```
**Correct Way**
```python
import string
from typing import List
def concatString(string_list: List[str]) -> str:
    return ''.join(string_list)
def main():
    string_list = list(string.ascii_letters * 100)
    for _ in range(10000):
        result = concatString(string_list)
main()
```
## 5.1 Cycle optimization
**Wrong way**
```python
def computeSum(size: int) -> int:
    sum_ = 0
    i = 0
    while i < size:
        sum_ += i
        i += 1
    return sum_
def main():
    size = 10000
    for _ in range(size):
        sum_ = computeSum(size)
main()
```
**Correct way**
```python
def computeSum(size: int) -> int:
    sum_ = 0
    for i in range(size):
        sum_ += i
    return sum_
def main():
    size = 10000
    for _ in range(size):
        sum_ = computeSum(size)
main()
```
## 5.3 For Calculations to reduce inner circulation
**Wrong Way**
```python
from math import sqrt
def main():
    size = 10000
    for x in range(size):
        for y in range(size):
            z = sqrt(x) + sqrt(y)
main()
```
**Correct Way**
```python
import math
def main():
    size = 10000
for x in range(size):
        sqrt_x = sqrt(x)
        for y in range(size):
            z = sqrt_x + sqrt(y)
main()
```
## 6. Use Numba.jit
```python
import numba
@numba.jit
def computeSum(size: float) -> int:
    sum = 0
    for i in range(size):
        sum += i
    return sum
def main():
    size = 10000
    for _ in range(size):
        sum = computeSum(size)
main()
```
