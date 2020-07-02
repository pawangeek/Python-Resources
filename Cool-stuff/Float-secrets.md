# Floating Points

## 1. Not Equal Alignments

```python 
>> a = (float('nan'),)
>> b = a

>> a, b 
Output: ((nan,), (nan,))

>> type(a), type(b)
Output: (<type 'tuple'>, <type 'tuple'>)

>> a == b
Output: True

>> a is b # (i.e. id(a)==id(b)
Output: True
  
>> a[0] == b[0]
Output: False
```
#### Parsing Rules

```python 
sign           ::= "+" | "-"
infinity       ::= "Infinity" | "inf"
nan            ::= "nan"
numeric_value  ::= floatnumber | infinity | nan
numeric_string ::= [sign] numeric_value
```

#### Inf Slice
```python 
>> a = (float('inf'),)
>> b = a

>> a # (inf,)
>> b # (inf,)

>> a == b # True
>> a is b # True
>> a[0] == b[0] # True
```

## 2. Abnormal Hash Results
```python 
>> a = float('inf')
>> b = float('inf')

>> c = float('nan')
>> d = float('nan')

>> a == b # True
>> c == d # False

>> hash(float('nan')) == hash(float('nan'))
Output: True
```

#### Dictionary Hash
```python 
>> a = {float('nan'): 1, float('nan'): 2}
>> a  # {nan: 1, nan: 2}

>> b = {float('inf'): 1, float('inf'): 2}
>> b  # {inf: 2}
```

