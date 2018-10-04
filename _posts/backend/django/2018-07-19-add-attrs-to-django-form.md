---
layout: post
title:  "Django: add attrs to django form"
categories: django
---



### 1. add attrs in widget

```python
number = forms.CharField(widget=forms.TextInput(attrs={'autocomplete': 'off', 'type': 'number'}))
```

### 2. use `__init__`

```python
class YourModelForm(forms.ModelForm):
    def __init__(self, *args, **kwargs):
        super(YourModelForm, self).__init__(*args, **kwargs)
        self.fields['number'].widget.attrs.update({
            'autocomplete': 'off',
            'type': 'number',
        })
```

### 3. use `Meta`

```python
class YourModelForm(ModelForm):
    class Meta:
        model = YourModel
        fields = ('number', 'title', 'birth_date')
        widgets = {
            'number': Textarea(attrs={'autocomplete': 'off', 'type': 'number'}),
        }
```

### 4. [django-widget-tweaks](https://github.com/jazzband/django-widget-tweaks)

{% raw %}
```html
{% load widget_tweaks %}
...
<div class="field">
   {{ form.number|attr:"autocomplete:off"|add_class:"my_css_class" }}
</div>
```
{% endraw %}
