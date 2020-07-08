# Development Tricks

## 01. View Source Code
```python
# demo.py
import inspect


def add(x, y):
    return x + y

print("===================")
print(inspect.getsource(add))
```

## 02. View package path

### Slow(pprint)
```python
import sys

from pprint import pprint   
pprint(sys.path)
```

### Faster
```python
# In terminal
# >> python3 -m site
```

## 03. Single line loop

### Nested one
```python
list1 = range(1,3)
list2 = range(4,6)
list3 = range(7,9)

for item1 in list1:
    for item2 in list2:
        for item3 in list3:
              print(item1+item2+item3)
```

### Using itertools.product
```python
from itertools import product

list1 = range(1,3)
list2 = range(4,6)
list3 = range(7,9)

for item1,item2,item3 in product(list1, list2, list3):
    print(item1+item2+item3)
```

## 04. Use the print output log
```python

>>> with open('test.log', mode='w') as f:
...     print('hello, python', file=f, flush=True)
>>> exit()

```
## 05. FUnction running time

### Using `time` (lame way)
```python
import time
start = time.time()

# run the function

end = time.time()
print(end-start)
```
### Using `timeit`
```python
import time
import timeit

def run_sleep(second):
    print(second)
    time.sleep(second)

print(timeit.timeit(lambda :run_sleep(2), number=5))
```

## 06. Using `lru_cache`

### `lru_cache`
```python
@functools.lru_cache(maxsize=None, typed=False)
```

### Example as decorator
```python
from functools import lru_cache

@lru_cache(None)
def add(x, y):
    print("calculating: %s + %s" % (x, y))
    return x + y

print(add(1, 2))
print(add(1, 2))
print(add(2, 3))
```

### Example for recursive call (Fibonacci)
```python
import timeit
from functools import lru_cache

@lru_cache(None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 2) + fib(n - 1)

print(timeit.timeit(lambda :fib(500), number=1))
# output: 0.0004921059880871326
```

## 07. Executing code before the program exits
```python
import atexit

@atexit.register
def clean():
  print("Hello")
  
def main():
  1/0
  
main()
```
## 08. To turn off the exception association
```python
try:
    print(1 / 0)
except Exception as exc:
    raise RuntimeError("bad thing").with_traceback(exc)
```
## 09. Delayed calls
```python
import contextlib

def callback():
    print('B')

with contextlib.ExitStack() as stack:
    stack.callback(callback)
    print('A')
```
## 10. Stream read files

### Using with...open...(x)
```python
with open("big_file.txt", "r") as fp:
    content = fp.read()
```

### Using readline as generator
```python
def read_from_file(filename):
    with open(filename, "r") as fp:
        yield fp.readline()
```

### In chunks
```python
def read_from_file(filename, block_size = 1024 * 8):
    with open(filename, "r") as fp:
        while True:
            chunk = fp.read(block_size)
            if not chunk:
                break

            yield chunk
```

### `functools.partial` to more optimization
```python
from functools import partial

def read_from_file(filename, block_size = 1024 * 8):
    with open(filename, "r") as fp:
        for chunk in iter(partial(fp.read, block_size), ""):
            yield chunk
```
