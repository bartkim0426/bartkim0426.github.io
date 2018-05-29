---
layout: post
title:  "python: check if variable exists in python"
categories: python
---


### 1. check if variable is in local

```python
if 'my_var' in locals():
```

### 2. check if variable is in global

```python
if 'my_var' in global():
```

### 3. check if object has attribute

```python
if hasattr(obj, 'attr_name'):
```

### 4. try/except

```python
try:
    my_var
except NameError:
    my_var = 'test'
```
