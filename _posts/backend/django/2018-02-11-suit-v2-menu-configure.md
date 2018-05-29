---
layout: post
title:  "django suit v.2.0 Menu configure"
categories: django
---

This is sample `SuitConfig` in django suit 2.0 demo app.

You can see demo app [here](http://v2.djangosuit.com/admin/login/?next=/admin/)

And the menu settings is right below. Just use `ParentItem` and `ChildItem`.

PLEASE CHECK that your app name is **lowercase** : (`demo.country`, not `demo.Country`). I spended hour to find this wrong. If something's wrong in this SuitConfig, it never shows error. So please check your spell well. 

```python
from suit.apps import DjangoSuitConfig
from suit.menu import ParentItem, ChildItem


class SuitConfig(DjangoSuitConfig):
    menu = (
        ParentItem('Content', children=[
            ChildItem(model='demo.country'),
            ChildItem(model='demo.continent'),
            ChildItem(model='demo.showcase'),
            ChildItem('Custom view', url='/admin/custom/'),
        ], icon='fa fa-leaf'),
        ParentItem('Integrations', children=[
            ChildItem(model='demo.city'),
        ]),
        ParentItem('Users', children=[
            ChildItem(model='auth.user'),
            ChildItem('User groups', 'auth.group'),
        ], icon='fa fa-users'),
        ParentItem('Right Side Menu', children=[
            ChildItem('Password change', url='admin:password_change'),
            ChildItem('Open Google', url='http://google.com', target_blank=True),

        ], align_right=True, icon='fa fa-cog'),
    )
```
