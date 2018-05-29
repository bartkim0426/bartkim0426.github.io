---
layout: post
title:  "Django: ModelChoiceField - prepopulate choice field with row from model"
categories: django
---

If you want to use choicefield from `Model` object, you can use `ModelChoiceField`.


```python
user = forms.ModelChoiceField(queryset=User.objects.filter(is_active=True))
```

Or you can use `ChoiceField` with `__init__` for change display.

```python
class UserForm(forms.Form):
    user = forms.ChoiceField(choices = [])

    def __init__(self, *args, **kwargs):
        super(UserForm, self).__init__(*args, **kwargs)
        self.fields[user].choices = [(u.phone, u.name) for u in User.objects.all()]
```
