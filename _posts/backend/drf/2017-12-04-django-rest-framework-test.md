---
layout: post 
title:  "DRF 테스트 (django rest framework)"
categories: drf
tags: django drf test
---

항상 개발을 하면서 테스트 코드를 짜야겠다고 생각만 하고 프로젝트 마감에 밀려 제대로 하지 못한 경우가 많았는데, 이번 프로젝트에서는 제대로 test code를 짜 보기로 결심했다.

django 테스트는 TestCase, pytest, model-mommy 등을 사용해서 해 본 적이 있는데, drf같은 REST API는 어떤 방식으로 테스트를 해야할 지 몰라 문서나 관련 자료를 찾아서 정리해보았다. 추후 테스트 코드를 짜면서 참고하면 될 듯 하다.


## [공식 문서](http://www.django-rest-framework.org/api-guide/testing/)
drf도 django만큼은 아니지만 문서에 test 관련된 정리가 꽤 잘 되어 있다.

### `APIRequestFactory`
test request를 만들어줌. `get`, `post`, `put` 등 모든 method에서 사용이 가능하다. 
```python
from rest_framework.test import APIRequestFactory

# Using the standard RequestFactory API to create a form POST request
factory = APIRequestFactory()
request = factory.post('/notes/', {'title': 'new idea'})
```

**`format` argument**도 사용 가능하다.
```python
# Create a JSON POST request
factory = APIRequestFactory()
request = factory.post('/notes/', {'title': 'new idea'}, format='json')
```


### Forcing authentication
request에 authentication을 주고 싶으면 `force_authenticate()` method를 사용 가능하다.
```python
from rest_framework.test import force_authenticate

factory = APIRequestFactory()
user = User.objects.get(username='olivia')
view = AccountDetail.as_view()

# Make an authenticated request to the view...
request = factory.get('/accounts/django-superstars/')
force_authenticate(request, user=user)
response = view(request)
```

이를 불러와서 다음과 같이 사용 가능하다.
```python
user = User.objects.get(username='olivia')
request = factory.get('/accounts/django-superstars/')
force_authenticate(request, user=user, token=user.auth_token)
```

### APIClient
django test에서 `client()`를 사용하듯이 drf에서도 `APIClient()`로 client를 사용 가능하다.
```python
from rest_framework.test import APIClient

client = APIClient()
client.post('/notes/', {'title': 'new idea'}, format='json')

# login도 가능
client.login(username="admin" password="secret")
client.logout()

# credentials method로 토큰 사용 가능
# Include an appropriate `Authorization:` header on all requests.
token = Token.objects.get(user__username='lauren')
client = APIClient()
client.credentials(HTTP_AUTHORIZATION='Token ' + token.key)
```


