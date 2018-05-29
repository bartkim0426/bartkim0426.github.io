---
layout: post
title:  "Django: custom filter for list_filter in admin"
categories: django
---


By extending `SimpleListFilter`, you can add custom filter to `list_filter`.

```python
from django.contrib.admin import SimpleListFilter

class CountryFilter(SimpleListFilter):
    title = 'country' # or use _('country') for translated title
    parameter_name = 'country'

    def lookups(self, request, model_admin):
        countries = set([c.country for c in model_admin.model.objects.all()])
        return [(c.id, c.name) for c in countries] + [
          ('AFRICA', 'AFRICA - ALL')]

    def queryset(self, request, queryset):
        if self.value() == 'AFRICA':
            return queryset.filter(country__continent='Africa')
        if self.value():
            return queryset.filter(country__id__exact=self.value())

class CityAdmin(ModelAdmin):
    list_filter = (CountryFilter,)
```
