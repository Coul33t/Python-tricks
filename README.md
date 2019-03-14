## 1) Iterate over a list pairwise
---
Let *l* be a list containing *n* elements. We want to run through it pairwise : **0/1, 1/2, ..., (n-1)/n**

e.g. `l = [0,1,2,3,4,5,6,7,8,9]` -> `1/2, 2/3, 3/4, ..., 8/9`

```
for a, b in zip(l[:-1], l[1:]):
    print(f'a = {a}, b = {b}')
```
or
```
from itertools import tee

def pairwise(iterable):
    a, b = tee(iterable)
    next(b, None)
    return zip(a, b)

for a, b in pairwise(l):
    print(f'a = {a}, b = {b}')
``` 
The latter is a bit faster than the former.

Time comparison:
```
    l = [x for x in range(1000000)]
    import time
    beg = time.time()
    for a, b in zip(l[:-1], l[1:]):
        pass
    print(f'zip time: {time.time() - beg}')
```
0.07999
```
    beg = time.time()
    for a, b in pairwise(l):
        pass
    print(f'pairwise time: {time.time() - beg}')
```
 0.07700
```
    beg = time.time()
    for val, i in enumerate(l):
        if i < len(l) - 1:
            a = val
            b = l[i+1]
    print(f'index time: {time.time() - beg}')
```
0.46799
```
    beg = time.time()
    limit = len(l) - 1
    for val, i in enumerate(l):
        if i < limit:
            a = val
            b = l[i+1]
    print(f'index time (pre-computed limit): {time.time() - beg}')
```
0.33899
