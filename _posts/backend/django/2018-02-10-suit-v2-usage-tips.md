---
layout: post
title:  "django suit v.2.0 후기 및 팁"
categories: django
---

이번 프로젝트에 django suit를 사용하면서, 2.0을 처음으로 사용해 보기로 하였다. 아직 공식적으로 release 되지는 않았지만, 이미 마무리 단계에 들어섰고 프로덕션 레벨에 사용할 정도의 수준은 되는 것 같아서 가벼운 admin에서 사용해 보기로 하였다.

2.0의 가장 큰 차이점은 모던화된 ui와 무려 모바일 최적화(!)이다. 모바일에서 장고 어드민을 사용할 수 있다니, 정말 어마어마하다.


우선 suit 2.0의 간략한 설치법을 정리하고 사용 중 발견한 팁들을 따로 공유하겠다. 

무엇보다 아직 2.0 공식 문서가 없기 때문에 demo app을 까보면서 하나하나 적용해 가야한다는 점이 조금 아쉽지만 이럴 때가 아니면 언제 suit를 까볼까 하는 생각에 아직은 즐겁게 2.0을 사용해보고 있다.


### 설치법
역시나 간단하다. `pip`로 설치 가능.

```python
pip install https://github.com/darklow/django-suit/tarball/v2
```

### 사용법
`suit 1`과 약간 달라졌다. 프로젝트 앱(메인 앱 등이 있으면 거기, 아무데나 해도 상관 없는 듯 하다)의 `apps.py`에 `SuitConfig`를 추가해 주고, 추가한 `SuitConfig`를 `INSTALLED_APPS`에 추가해준다.

```python
# my_project_app/apps.py
from suit.apps import DjangoSuitConfig

class SuitConfig(DjangoSuitConfig):
    layout = 'horizontal'
```

이후 INSTALLED_APPS에 추가할 때, 반드시 `django.contrib.admin` 위에 설치해주어야 한다.
```python
NSTALLED_APPS = (
    ...
    'my_project_app.apps.SuitConfig',
    'django.contrib.admin',
)
```

이렇게 하면 기본적인 suit가 설치가 된다. 물론 기본 기능과 ui가 훌륭하지만, 사소하게 변경해야 할 것들 (admin 이름 등)이 많은데 현재로서 문서가 없기 때문에 코드를 찾아보면서 해야한다. 내가 진행하면서 발생한 문제와 해결 방법은 앞으로 하나하나 블로그 글에 정리할 생각이다.
