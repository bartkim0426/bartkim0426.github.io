---
layout: post
title:  "python: make flat list"
categories: python
---


### usling list comprehension & lambda

```python
# list comprehension
flat_list = [item for sublist in l for item in sublist]
# labmda
flatten = lambda l: [item for sublist in l for item in sublist]
```

### using itertools.chain()

```
import itertools
flat_list = list(itertools.chain.from_iterable(l))
```

### using sum
super easy but not inefficient way

```python
flat_list = sum(l, [])
```

### using reduce

```python
from functools import reduce
flat_list = reduce(lambda x, y: x + y, l)

# or using operator
import operator
flat_list = reduce(operator.concat, l)
```

**cons**
- can use only one-depth list 
- can use only list that have list (not in flat list)


For solving that cons, you can make your own def like `_flat`

```python
from collections import Iterable

def _flat(items):
    for x in items:
        if isinstance(x, Iterable) and not isinstance(x, (str, bytes)):
            yield from _flat(x)
        else:
            yield x

flat_list = list(_flat(l))
```
