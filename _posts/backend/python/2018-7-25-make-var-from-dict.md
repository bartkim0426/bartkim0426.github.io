---
layout: post
title:  "python: make variable from dict"
categories: python
---


## Use `exec`

```python
d = {'a':1, 'b':2}
for key,val in d.items():
    exec(key + '=val')

# one line
list(map(exec, ("{key}={value}".format(key=x[0], value=x[1]) for x in d.items())))
```

## Using `Namespace`

```python
from argparse import Namespace

d = {'a':1, 'b':2}
n = Namespace(**d)
```
