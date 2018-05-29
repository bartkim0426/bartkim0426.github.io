---
layout: post
title:  "django suit v.2.0 change site_header"
categories: django
---

Currently I'm using django suit v.2 to new project. It has really great features and ui, but not has documentation yet.

I spended some time to find the way to change site_header (default is `django Administration`) and I find it. 

Here's part of suit's `base.html`

you can see `branding` in this code. That one is for django site header. It use `site_header` context if exists, and use `Django administration` by default. 

{% raw %}
```html
{% extends 'admin/base.html' %}
{% load i18n admin_static suit_tags %}

{% block stylesheet %}{% static "suit/css/suit.css" %}{% endblock %}

{% block extrastyle %}
    <link href='https://fonts.googleapis.com/css?family=Roboto:400,100,300,500,700,900' rel='stylesheet' type='text/css'>
    <link href='{% static "suit/css/font-awesome.min.css" %}' rel='stylesheet' type='text/css'>
{% endblock %}

{% block bodyclass %}{{ block.super|suit_body_class:request }}{% endblock %}

{% block title %}{{ title }} | {{ site_title|default:_('Django site admin') }}{% endblock %}

{% block branding %}
    <h1 id="site-name">
        <a href="{% url 'admin:index' %}">
            {{ site_header|default:_('Django administration') }}
            <span class="header-label">{% trans 'Admin' %}</span></a>
    </h1>
{% endblock %}
...
```
{% endraw %}

So if you want to change this, you simplely send `site_header` to admin context. In your `admin.py` (maybe your main app, or any app possible), you can add `admin.site.site_header`. Done. Easy, right?

```python 
from django.contrib import admin

admin.site.site_header = 'My site header'
```


> I found this in suit 2.0 demo project's admin.py. It's just demo app, but has many admin features so if you're interested in django admin and suit 2.0, please check demo app code.

> [here's](https://github.com/darklow/django-suit/blob/v2/demo/demo/admin.py#L19) the code.
