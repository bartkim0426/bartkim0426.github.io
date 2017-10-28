---
layout: post
title:  "django-hitcounts: 페이지 view를 쉽게 이용하자"
categories: django django-hitcounts
---


## 설치법
여느때와 같이 pip를 이용해 설치해주자.

```
pip install django-hitcount
```

Installed app에 넣는것을 까먹으면 안된다
```
# settings.py
INSTALLED_APPS = (
    ...
	'hitcount'
)
```

## DetailView, Model에 추가하기
CBV (DetailView)를 사용해서 detail page를 만들어 놓았다면, 아주 쉽게 hit 수를 카운팅할 수 있다. (만약 fbv로 구현이 되어있다면 이번 기회에 CBV를 사용해보자. 좋다.)


```
# models.py
from django.db import models

from hitcount.models import HitCountMixin

# 다음과 같이 HitCountMixin을 상속받아주자
class MyModel(models.Model, HitCountMixin):
	pass

# shell에서 다음과 같이 불러오면 된다
my_model = MyModel.objects.first()
my_model.hit_count.hits
# 다음과 같이 특정 day를 지정도 가능하다!
my_model.hit_count.hits_in_last(days=7)
```

다음은 DetailView. view도 마찬가지로 `HitCountDetailView`를 상속받은 이후 `count_hit = True`를 적어주면 완료된다.


```
# views.py
 
from hitcount.views import HitCountDetailView

class PostCountHitDetailView(HitCountDetailView):
	model = Post        # your model goes here
	count_hit = True    # set to True if you want it to try and count the hit
```

## Temploate tag
그냥 `{{ object.hit_count.hits }}`로 불러올 수도 있지만, 템플릿 태그를 사용해서 더 쉽게 불러올 수 있따


```
# remember to load the tags first
{% load hitcount_tags %}

# Return total hits for an object:
{% get_hit_count for [object] %}

# Get total hits for an object as a specified variable:
{% get_hit_count for [object] as [var] %}

# Get total hits for an object over a certain time period:
{% get_hit_count for [object] within ["days=1,minutes=30"] %}

# Get total hits for an object over a certain time period as a variable:
{% get_hit_count for [object] within ["days=1,minutes=30"] as [var] %}
```


## 사용평
count를 구현하는 코드는 어렵지 않지만, 구현하다 보면 은근히 신경써야 할 내용들이 많다. django-hitcount는 github star 수가 많지 않아 조금 걱정했지만 원활하게 잘 동작한다. 한번 사용해봄직 하다.


더 자세한 내용은 [django-hitcount 공식문서](http://django-hitcount.readthedocs.io/en/latest/index.html) 또는 [github code](https://github.com/thornomad/django-hitcount/blob/master/docs/index.rst)를 참고
