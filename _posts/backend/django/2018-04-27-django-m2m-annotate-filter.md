---
layout: post
title:  "Django: filter queryset with m2m - using annotate"
categories: django
---


When filter queryset using `ManyToManyField`, it takes too much sql queries.

For avoid this situation, we can use `annotate`.

### `Count`
With `django.db.models.Count`, you can count number of `m2m` objects.

For example, if want to filter queryset having more than one `ManyToManyField` called `product` (for example)

```python
from django.db.models import Count
...
queryset = queryset.annotate(product_num=Count('product')).filter(price_num__gte=2)
...
```

### `Min`, `Max`

It gives `m2m` object with minimum or maximum value.

If you want to filter queryset with `m2m` product having biggest price,

```python
from django.db.models import Min, Max
...
queryset = queryset.annotate(max_price=models.Min('product__price')).filter(max_price__gte=50000)
```
