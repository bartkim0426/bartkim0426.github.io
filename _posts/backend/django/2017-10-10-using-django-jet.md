---
layout: post
title:  "Django admin을 더 풍성하게, django jet"
categories: django
tags: django jet admin
---

Django의 가장 큰 장점 중 하나는 admin이 기본으로 아주 잘 되어 있다는 것이다. 직접 구현하려면 아주 큰 노력이 필요한 admin을 장고에서는 기본으로 제공하고, 다양한 기능 또한 제공한다. 

아쉬운 점은 admin이 django library 내부에 있기 때문에 customizing이 조금 힘들다는 점인데, django suit, django jet 등 다양한 admin 라이브러리를 사용하면 이 또한 충분히 상쇄가 가능하다.


지금까지는 django suit만 쭉 사용해봤는데, modern browser와 잘 맞지 않는 느낌, 약간의 interface의 아쉬움 말고는 정말 잘 구성되어있다. 뿐만 아니라 django filer와의 호환이 잘 맞지 않아서 다른 라이브러리를 찾아보았다. 

[Django jet](https://github.com/geex-arts/django-jet)는 반응형으로 만들어진 admin library로 (아주 잠깐) 써 보았지만 완성도가 훌륭하고, 특히 모던 브라우저 사이트의 어드민으로 사용하기에 손색이 없다. 다만 통계, 재고 등 많은 정보를 담기에는 ui가 조금 아쉬운 면이 있다. Live Demo는 [이곳](http://demo.jet.geex-arts.com/admin/login/?next=/admin/)에서 확인이 가능하다.

> 현재 suit v2가 개발단계에 있고, 사용해보니 상당히 모던하게 잘 구성되어 있는데 아직 개발 단계라 제대로 사용해보기는 조금 애매하다. 자세한 내용은 [django suit v2](http://django-suit.readthedocs.io/en/v2/)를 참고하면 된다. (suit 관련 글은 추후 올릴 예정)


## Django JET 설치
설치법은 매우 간단하다. 
```python
pip install django-jet
```

다음과 같이 pip로 설치해 준 뒤 INSTALLED APP에 `jet`를 추가해준다. (다음과 같이 `django.contrib.admin` 전에 와야한다.)

jet의 큰 장점인 admin main 화면의 dashboard를 사용하려면 따로 추가를 해줘야한다.

```python
INSTALLED_APPS = (
    ...
    'jet',
	# dashboard 기능을 사용하려면 다음을 추가
    'jet.dashboard',
    'django.contrib.admin',
)
```

그리고 `urls.py`에 다음을 추가해준다.

```python
urlpatterns = patterns(
    '',
    url(r'^jet/', include('jet.urls', 'jet')),  # Django JET URLS
    url(r'^jet/dashboard/', include('jet.dashboard.urls', 'jet-dashboard')),  # Django JET dashboard URLS
    url(r'^admin/', include(admin.site.urls)),
    ...
)
```
이후 `python manage.py migrate`를 해주면 admin 사용이 가능하다. (www.eaxample.com/admin으로 사용 가능)

### 다양한 config
jet는 쉽고 간편한 다양한 설정들을 제공한다. 기본적으로 여러 테마를 제공할 뿐 아니라 대시보드, 메뉴 등의 설정을 할 수 있따. 전체 설정은 [jet documentation](http://jet.readthedocs.io/en/latest/configuration.html)에서 확인 가능하다. 
