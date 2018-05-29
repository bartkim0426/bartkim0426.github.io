---
layout: post
title:  "Django: custom list_filter in ModelAdmin"
categories: django
---

Using `list_filter` in `ModelAdmin`, ordering follow `model`'s default `ordering` in `Meta`. 

But if you want to have custom filter, you can add some filter inheriting `SimpleListFilter`


For exmple, it will automatically order by `created_at` (reversd, for `-`)
```python
class MyModel(models.Model):
    class Meta:
        ordering = ['-created_at']
```

When you want to filter by `status`, you can add your own custom filter.
```python
class StatusFilter(SimpleListFilter):
    title = _('status')
    parameter_name = 'status'

    def lookups(self, request, model_admin):
        queryset = model_admin.queryset(request)
        return [(i, i) for i in queryset.value_list('status', flat=True).distinct().order_by('status')]

    def queryset(self, request, queryset):
        if self.value():
            return queryset.filter(statu__exact=self.value())

class MyModelAdmin(ModelAdmin):
    list_filter = (StatusFilter,)
```
