---
layout: post
title:  "Django: add custom button in admin"
categories: django
---


## Add button in admin change list


### 1. Add by action

```python
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')
make_published.short_description = "Mark selected stories as published"

class ArticleAdmin(admin.ModelAdmin):
    list_display = ['title', 'status']
    ordering = ['title']
    actions = [make_published]
```

> docs [here](https://docs.djangoproject.com/en/2.0/ref/contrib/admin/actions/),

### 2. Add custom button

Extending the `change_form.html` for specific app's template dir. (Or whole admin page)

> Or just add html inside `templates/admin/myapp/change_form.html` and it will applied automatically.


{% raw %}
```html
{% extends 'admin/change_form.html' %}
{% block submit_buttons_bottom %}
    {{ block.super }}
    <div class="submit-row">
            <input type="submit" value="Make Unique" name="_make-unique">
    </div>
{% endblock %}
```
{% endraw %}

```python
@admin.register(Villain)
class VillainAdmin(admin.ModelAdmin, ExportCsvMixin):
    ...
    change_form_template = "entities/villain_changeform.html"


    def response_change(self, request, obj):
        if "_make-unique" in request.POST:
            matching_names_except_this = self.get_queryset(request).filter(name=obj.name).exclude(pk=obj.id)
            matching_names_except_this.delete()
            obj.is_unique = True
            obj.save()
            self.message_user(request, "This villain is now unique")
            return HttpResponseRedirect(".")
        return super().response_change(request, obj)
```

## Add button in admin change detail view page


### 1. Using template tags

Make custom `submit_row` template tag and use it instead of `submit_row` in django template.

```python
@register.inclusion_tag('path/to/your/custom_submit_line.html', takes_context=True)
def custom_submit_row(context):
    """
    Displays the row of buttons for delete and save.
    """
    opts = context['opts']
    change = context['change']
    is_popup = context['is_popup']
    save_as = context['save_as']
    ctx = {
        'opts': opts,
        'show_delete_link': (
            not is_popup and context['has_delete_permission'] and
            change and context.get('show_delete', True)
        ),
        'show_save_as_new': not is_popup and change and save_as,
        'show_save_and_add_another': (
            context['has_add_permission'] and not is_popup and
            (not save_as or context['add'])
        ),
        'show_save_and_continue': not is_popup and context['has_change_permission'],
        'is_popup': is_popup,
        'show_save': True,
        'preserved_filters': context.get('preserved_filters'),
    }
    if context.get('original') is not None:
        ctx['original'] = context['original']
    return ctx
```


And customize your `submit_row`

{% raw %}
```html
{% if save_on_top %}{% block submit_buttons_top %}{% custom_submit_row %}{% endblock %}{% endif %}
...
{% block submit_buttons_bottom %}{% custom_submit_row %}{% endblock %}
```
{% endraw %}

> [SO answer](https://stackoverflow.com/questions/34897388/django-how-to-add-a-custom-button-to-admin-change-form-page-that-executes-an-ad)

