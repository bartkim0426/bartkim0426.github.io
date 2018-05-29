---
layout: post
title:  "Django: django channels basic usage"
categories: django
---


I have a chance to make chat service in my company. 

It's my first time to make async web by django, so it's very excited things.


I found many chat libraries that support django, but I choolse [`django channels`](https://channels.readthedocs.io/en/latest/).

It's well made and well managed library. Also, django channels adopted as an official django project - It did'nt included in django 1.10 for a variety of reasons, but django officially supports django channels, so it work perfectly in current django releases (Django 1.11 or 2.0).

> In [here](https://www.djangoproject.com/weblog/2016/sep/09/channels-adopted-official-django-project/), you can read articles about it.


> There's lot of difference between `channels 1.x.x` and `channels 2.x.x` (like usage for `routing.py`). This is for `channels 2.x.x`.


### Install

Installation is easy. Using `pip`

```bash
pip install channels
```

And add `channels` to `INSTALLED_APPS` settings:
```python
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    ...
    'channels',
)
```


Then make `myproject/routing.py`

Make default routing in project dir (same directory that `wsgi.py` and `settings.py` exists)
```python
from channels.auth import AuthMiddlewareStack
from channels.routing import ProtocolTypeRouter, URLRouter
from chat import routing


application = ProtocolTypeRouter({
    # (http->django views is added by default)
    'websocket': AuthMiddlewareStack(
        URLRouter(
            routing.websocket_urlpatterns
        )
    ),
})
```

Add `ASGI_APPLICATION` to settings.

```python
ASGI_APPLICATION = "myproject.routing.application"
# use path that your routing.py exists
# it's same as WSGI_APPLICATION path
# for example "config.routing.application" when my WSGI_APPLICATION settings are below
# WSGI_APPLICATION = 'config.wsgi.application'
```

### Making `chat` app
```bash
python manage.py startapp chat
```

and add it to `INSTALLED_APPS`
```python
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    ...
    'channels',
    ...
    'chat',
)
```

### Add `consumers.py`
Then you to add `consumers.py`. I added `WebsocketConsumer`.

```python
# chat/consumers.py
from asgiref.sync import async_to_sync
from channels.generic.websocket import WebsocketConsumer
# from channels.auth import login
import json


class ChatConsumer(WebsocketConsumer):
    def connect(self):
        self.room_name = self.scope['url_route']['kwargs']['room_name']
        self.room_group_name = 'chat_%s' % self.room_name

        # Join room group
        async_to_sync(self.channel_layer.group_add)(
            self.room_group_name,
            self.channel_name
        )

        self.accept()

    def disconnect(self, close_code):
        # Leave room group
        async_to_sync(self.channel_layer.group_discard)(
            self.room_group_name,
            self.channel_name
        )

    # Receive message from WebSocket
    def receive(self, text_data):
        text_data_json = json.loads(text_data)
        message = text_data_json['message']

        # Send message to room group
        async_to_sync(self.channel_layer.group_send)(
            self.room_group_name,
            {
                'type': 'chat_message',
                'message': message
            }
        )

    # Receive message from room group
    def chat_message(self, event):
        message = event['message']

        # Send message to WebSocket
        self.send(text_data=json.dumps({
            'message': message
        }))
```

### Add `views` and `urls`

When make a `views`, you can freely make anyway you want. I just added them from tutorials - that `chat` list and `chat` detail view.


in `views.py` in `chat` app,

```python
from django.shortcuts import render
from django.utils.safestring import mark_safe
import json

from .models import Room


def chat(request):
    return render(request, 'chat/index.html', {})


def room(request, room_name):
    return render(request, 'chat/room.html', {
        'room_name_json': mark_safe(json.dumps(room_name))
    })
```

In `urls.py` in `chat` app,

```python
# chat/urls.py
from django.conf.urls import url

from gcfapp.chat.views import (
    room,
    chat,
    chat_room,
)

urlpatterns = [
    url(r'^$', chat, name='chat'),
    url(r'^(?P<name>[^/]+)/$', chat_room, name='room'),
]
```



### Add `chat/index.html` and `chat/room.html` with javascript

`chat/index.html`

```html
<!-- chat/templates/chat/index.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>Chat Rooms</title>
</head>
<body>
    What chat room would you like to enter?<br/>
    <input id="room-name-input" type="text" size="100"/><br/>
    <input id="room-name-submit" type="button" value="Enter"/>
</body>
<script>
    document.querySelector('#room-name-input').focus();
    document.querySelector('#room-name-input').onkeyup = function(e) {
        if (e.keyCode === 13) {  // enter, return
            document.querySelector('#room-name-submit').click();
        }
    };

    document.querySelector('#room-name-submit').onclick = function(e) {
        var roomName = document.querySelector('#room-name-input').value;
        window.location.pathname = '/chat/' + roomName + '/';
    };
</script>
</html>
```

`chat/room.html`

```html
<!-- chat/templates/chat/room.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>Chat Room</title>
</head>
<body>
    <textarea id="chat-log" cols="100" rows="20"></textarea><br/>
    <input id="chat-message-input" type="text" size="100"/><br/>
    <input id="chat-message-submit" type="button" value="Send"/>
</body>
<script>
    var roomName = '{{ room }}';

    var chatSocket = new WebSocket(
        'ws://' + window.location.host +
        '/ws/chat/' + roomName + '/');

    chatSocket.onmessage = function(e) {
        var data = JSON.parse(e.data);
        var message = data['message'];
        document.querySelector('#chat-log').value += (message + '\n');
    };

    chatSocket.onclose = function(e) {
        console.error('Chat socket closed unexpectedly');
    };

    document.querySelector('#chat-message-input').focus();
    document.querySelector('#chat-message-input').onkeyup = function(e) {
        if (e.keyCode === 13) {  // enter, return
            document.querySelector('#chat-message-submit').click();
        }
    };

    document.querySelector('#chat-message-submit').onclick = function(e) {
        var messageInputDom = document.querySelector('#chat-message-input');
        var message = messageInputDom.value;
        chatSocket.send(JSON.stringify({
            'message': message
        }));

        messageInputDom.value = '';
    };
</script>
</html>
```


Then when you `runserver`, you can access `localhost:8000/chat/` and make a simple chat room.


### Following todo
- Add models (`Room`, `Message`) to save them with django orm
- Attach `consumers` with exist django `User` models
- File attach while chat
