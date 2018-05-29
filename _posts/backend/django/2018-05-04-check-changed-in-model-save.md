---
layout: post
title:  "Django: check changed fields in Model save() method"
categories: django
---

```python
def save(self, *args, **kw):
    if self.pk is not None:
        orig = MyModel.objects.get(pk=self.pk)
        field_names = [field.name for field in MyModel._meta.fields]
        fields_stats = {}
        for field_name in field_names:
            fields_stats[field_name] = getattr(orig, field_name) != getattr(self, field_name)
    super(MyModel, self).save(*args, **kw)
```

Then `field_stats` will be 

```
{
    'field_name1': True,
    'field_name1': False,
}
```
