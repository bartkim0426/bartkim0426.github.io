---
layout: post
title:  "Django: architect - enhance ORM for make multiple table"
categories: django
---


Recently, I made chat service with `django channels`. It's not temporary chat like CS, so I have to save and load all chat logs and messages.


First, I try to use `redis` to save those data. 


But it's not that big project, and I want to use django ORM to those data.


I found good library to make multiple table per one model. I want to make multiple tables for `Message` model.


[architect](https://github.com/maxtepkeev/architect) enhances ORMs with more feature. 

### Installing

```
pip install architect
```


### Table partitioning

Which `architect`, we can partition sql tables for each model.

### Add model

```python
import architect

@architect.install('partition', **options)
class Model(object):
    pass
```

For options, we can use 

- type(required) : range, list, etc...
- subtype(required) : range, date, integer, string_firstchars, string_lastchars
- contraint(required)
- column(required)
- db (optional) : `Django` or `SQLAlchemy`

i.e. if want to divide by `ForeignKey` id,


```python
@architect.install('partition', type='range', subtype='integer', constraint='1', column='room_id')
class Message(models.Model):
    room = models.ForeignKey(Room, related_name='messages')
    message = models.TextField()
```

### Usage

Then we can be accessed via `Model.architect.partition`.

also operation with `Model.architect.operation` command.

For more information, check [official docs](http://architect.readthedocs.io/features/operation.html)
