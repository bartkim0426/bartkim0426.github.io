---
layout: post
title:  "Django: get all logged-in users by using session"
categories: django
---


In django, you can know all logged in users by `Session` for non-expired sessions.

For that, just make `get_all_logged_in_users`. (maybe in `function.py` or somewhere else.)

```python
# If you use custom user, import it instead of User
from django.contrib.auth.models import User
from django.contrib.sessions.models import Session
from django.utils import timezone


def get_all_logged_in_users():
    sessions = Session.objects.filter(expire_date__gte=timezone.now())
    uid_list = []

    for session in sessions:
        data = session.get_decoded()
        uid_list.append(data.get('_auth_user_id', None))

    return User.objects.filter(id__in=uid_list)
```

In `views.py`, you can call `get_all_logged_in_users` and send it by `context`

```python
from django.shortcuts import render
from django.contrib.auth.models import User


def chat_room(request):
    ...
    users = User.objects.all()
    logged_users = get_all_logged_in_users()
    ...

    return render(request, 'chat/room.html', {
        'users': users,
        'logged_users': logged_users,
    })
```

Then you can use `logged_users` in your template.

{% raw %}
```html
<ul class="user-list">
    {% for user in users %}
        {% if user in logged_users %}
            <li class="exist">{{ user }}</li>
        {% else %}
            <li>{{ user }}</li>
        {% endif %}
    {% endfor %}
</ul>
```
{% endraw %}
