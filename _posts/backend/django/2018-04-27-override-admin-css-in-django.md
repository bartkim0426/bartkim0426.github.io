---
layout: post
title:  "Django: overriding admin css in django"
categories: django
---


### Method 1: Override admin template

In django admin templates, there's 
{% raw %}
```
{% block extrastyle %}{% endblock %}
```
{% endraw %}
and can add some styles to there.



### Method 2: Add Media class to ModelAdmin

If you want to add `css` or `js` for only one ModelAdmin, you can add `Media` class to `ModelAdmin`

```python
class CustomModelAdmin(admin.ModelAdmin):
    class Media:
        js = ('js/admin/my_own_admin.js',)    
        css = {
             'all': ('css/admin/my_own_admin.css',)
        }
```
