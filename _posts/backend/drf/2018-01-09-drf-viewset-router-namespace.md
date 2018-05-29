---
layout: post 
title:  "django rest framework router의 namespace"
categories: drf
tags: django drf test
---


`Django rest framework`에서 `viewset`을 사용하는 경우 따로 url name 을 지정하지 않기 때문에 `reverse`등을 사용할 때 url namespace와 name을 알기가 힘들었다.

그래서 소스 코드를 보던 중 자동으로 url name을 생성해주는 것을 발견해서 공유한다.


`viewset`을 사용하면 `list`와 `detail` 크게 두가지의 url이 생성된다. 
간단하게 다음과 같은 url name을 입력하면 해당 viewset의 list url을 알 수 있다.
```
# list의 경우
'<url_namespace>:<base_name>-list'
# detail의 경우
'<url_namespace>:<base_name>-detail'
```

- `url_namespace`: core url(base urls.py)에 적은 namespace
- `base_name`: viewset을 `router`에 등록할 때 쓴 `base_name`
예를 들면 다음 `urls.py`를 쓴다면 `api:posts-list`를 name으로 사용하면 된다.
```python
url(r'^api/', include('api.urls', namespace='api')),
...
router.register(r'', PostViewSet, base_name='posts')
```

### 실제로 `reverse()`에서 사용할 때

```python
> reverse('api:posts-list')
/api/posts
> reverse('api:posts-list', kwargs={'pk': 1})
/api/posts/1
```
