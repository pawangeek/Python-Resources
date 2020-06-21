# Built-in Functions

## 1. set()
**Using sets**
```python
obj = ['a','b','c','b','a'] 

print(set(obj)) 
# output：{'b', 'c', 'a'} 
```
**union, intersection and set difference**
```python
A = set('hello') 
B = set('world') 
 
A.union(B) 
# output：{'d', 'e', 'h', 'l', 'o', 'r', 'w'} 

A.intersection(B) 
# output：{'l', 'o'} 

A.difference(B)
# output：{'d', 'r', 'w'} 
```

## 2. eval()
**String expression**
```python
a = eval('[1,2,3]') 
print(type(a)) 
# output：<class 'list'> 
 
b = eval('max([2,4,5])') 
print(b) 
# output： 5 
```
**Methods and functions**
```python
def divide_by_two(x):
  return x//2
  
eval('divide_by_two(8)')
# output: 4
```
## 3. sorted()
**Ascending order**
```python
a = sorted([2,4,3,7,1,9]) 

print(a) 
# output：[1, 2, 3, 4, 7, 9] 
```
**Reverse**
```python
sorted((4,1,9,6),reverse=True) 

print(a) 
# output：[9, 6, 4, 1] 
```
**By length as parameter**
```python
chars = ['apple','papaya','pear','banana'] 
a = sorted(chars,key=lambda x:len(x)) 

print(a) 
# output：['pear', 'apple', 'papaya', 'banana'] 
```
**Custom rules**
```python
tuple_list = [('A', 1,5), ('B', 3,2), ('C', 2,6)] 
a = sorted(tuple_list, key=lambda x: x[1]) 

print(a) 
# output：[('A', 1, 5), ('C', 2, 6), ('B', 3, 2)] 
```
## 4. reversed()
```python
a = reversed('abcde') 
print(list(a)) 
# output：['e', 'd', 'c', 'b', 'a'] 
 
b = reversed([2,3,4,5]) 
print(list(b)) 
# output：[5, 4, 3, 2] 
```
## 5. map()
**Uppercase**
```python
chars = ['apple','papaya','pear','banana'] 
a = map(lambda x:x.upper(),chars) 

print(list(a)) 
# output：['APPLE', 'PAPAYA', 'PEAR', 'BANANA'] 
```
**Square each number**
```python
nums = [1,2,3,4] 
a = map(lambda x:x*x,nums) 

print(list(a)) 
# output：[1, 4, 9, 16] 
```
## 6. bin()
```python
number = 5

print(bin(number))
# output: 0b101
```
## 7. filter()
**Remove even**
```python
nums = [1,2,3,4,5,6] 
a = filter(lambda x:x%2!=0,nums) 

print(list(a)) 
# output：[1,3,5] 
```
**word starts with 'b'**
```python
chars = chars = ['apple','papaya','pear','banana'] 
a = filter(lambda x:'b' in x,chars) 

print(list(a)) 
# output：['banana'] 
```
## 8. enumerate()
**Enumeration example**
```python
chars = ['apple','papaya','pear','banana'] 
for i,j in enumerate(chars): 
    print(i,j) 
 
''' 
output： 
0 apple 
1 papaya
2 pear 
3 banana 
''' 
```
**Enumerating strings**
```python
a = enumerate('abc') 

print(list(a)) 
# output：[(0, 'a'), (1, 'b'), (2, 'c')]
```
**Using next()**
```python
chars = ['apple', 'papaya', 'pear', 'banana']
enumerate_object = enumerate(chars)

next(enumerate_object)
# output: (0,'apple')

next(enumerate_object)
# output: (1,'papaya')
```
