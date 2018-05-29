---
layout: post
title:  "django test plus"
categories: django
---


### Install
```bash
pip install django-test-plus
```

### Usage
```python
from test_plus.test import TestCase

class MyTestCase(TestCase):
    ...
```


### Methods

#### `reverse(url_name, *args, **kwargs)`
Can use `reverse` easily
```python
def test_sth(self):
    url = self.reverse('my-url-name', pk=12)
```

#### Http GET methos
```python
def test_get_url(self):
    response = self.get('url-name')
    # When using django testcase
    respone = self.client.get(reverse('my-url-name'))
    self.assertEqual(response.context['test'], 1)
    # became easy
    respone = self.get(reverse('my-url-name'))
    self.respone_200()
    # can pass query : /search/?query=testing
    self.get('search', data={'query': 'testing'})
```

#### Http POST methos
```python
def test_post_url(self):
    respone = self.post(
        'my-url-name',
        data={
            'title': 'test title',
        }
    )
```
