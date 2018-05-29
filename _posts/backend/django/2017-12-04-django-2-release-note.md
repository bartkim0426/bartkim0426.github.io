---
layout: post
title:  "Django 2.0 release note: Simple URL"
categories: django
tags: django 2.0
---

드디어 django 2.0이 정식 release 됐다. 이제 `pip install django`로 장고를 설치하면 2.0 버전이 설치된다. 1.11이 나온게 엊그제같은데.. 정말 빠르다.

[2.0 relaease note](https://docs.djangoproject.com/en/2.0/releases/2.0/)를 쭉 읽어 봤는데 다른 변화들도 많지만 무엇보다도 url에서 엄청난 변화가 생겼다.

주목할만한 변화 몇 가지만 남겨놓아본다.


## Simplified URL routing syntax
기존에
```python
url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
```
로 사용하던 `django.urls.path()`라는 새로운 기능을 사용해 간단하게 사용 가능하다.

```python
path('articles/<int:year>', views.year_archive),
```
자주 사용해도 정규표현식과 $, ^ 등으로 여러 번 헷갈린 적이 있는데, 간편화되서 좋기도 하지만 앞으로 꽤 오랜 시간 동안 두가지가 같이 사용되서 조금의 혼란을 야기할 것 같다. 

django는 `path` 사용을 권장하고 이전 방식을 사용하려면 `re_path`를 사용해야한다.
```python
from django.urls import include, re_path

urlpatterns = [
    re_path(r'^index/$', views.index, name='index'),
    re_path(r'^bio/(?P<username>\w+)/$', views.bio, name='bio'),
    re_path(r'^weblog/', include('blog.urls')),
    ...
]
```

새로운 방식은 다음과 같이 사용하면 된다 
```python
from django.urls import path

from . import views

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<int:year>/', views.year_archive),
    path('articles/<int:year>/<int:month>/', views.month_archive),
    path('articles/<int:year>/<int:month>/<slug:slug>/', views.article_detail),
]
```
path만 불러온 후, 정규표현식 없이 `<converter:name>` 형식으로 사용이 가능하다.

converter의 종류는 
- str: `/`를 제외한 모든 문자열
- int
- slug: hypen, undersocre로 연결된 slug 형태의 str
- uuid
- path

이 외 자신의 custom path converter를 등록해서 사용 가능하다. 다음 세가지를 포함해야하는데
- `regex` 클래스
- `to_python(self, value)`
- `to_url(self, value)`

```python
class FourDigitYearConverter:
    regex = '[0-9]{4}'

    def to_python(self, value):
        return int(value)

    def to_url(self, value):
        return '%04d' % value
```
만들어진 Converter를 `register_converter()`를 사용해서 urlconf에 등록해준다.
```python
from django.urls import register_converter, path

from . import converters, views

# 이런식으로 등록한다. 등록한 converter를 호출해서 사용하면 됨
register_converter(converters.FourDigitYearConverter, 'yyyy')

urlpatterns = [
    path('articles/2003/', views.special_case_2003),
    path('articles/<yyyy:year>/', views.year_archive),
    ...
]
```

이 외 나머지는 거의 동일하며, nested 된 url 등은 `re_path`를 사용하여 이전 방식으로 사용해야 한다.

## Mobile-friendly contrib.admin
django admin이 이제 모든 major mobile device 에서 반응형 페이지를 지원한다! 

사실 많은 사람들이 suit, jet 등 admin library를 사용할텐데, 그런 라이브러리들에서 2.0 지원이 나오면 장고 어드민이 더욱 강력해질 것으로 보인다. 추후 2.0 어드민을 한 번 사용 해봐야지.
