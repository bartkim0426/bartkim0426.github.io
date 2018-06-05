---
layout: post
title:  "python: make uuid in python"
categories: python
---

### What's uuid

> UUID(Universal Unique Identifier) is a 128-bit number used to identify information in computer systems.
> - [wikipedia 'UUID'](https://en.wikipedia.org/wiki/Universally_unique_identifier)

It represented as 32 hexadecimal digits, displayed in five groups separated by hyphens, form `8-4-4-4-12` for total of 36 characters. 

```
022db29c-d0e2-11e5-bb4c-60f81dca7676
```


### Make uuid in python

```python
import uuid
print uuid.uuid4()
```


### Use uuid in django

django support `UUIDField` for easy using uuid.

```python

...
uuid = models.UUIDField(
    default=uuid.uuid4,
    editable=False,
    unique=True,
)
...
```

