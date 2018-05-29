---
layout: post
title:  "django: 기본 비밀번호 validation 수정하기"
categories: django
---

django는 auth 기능을 통해서 강력한 유저/비밀번호 기능을 제공한다. 아이디와 유사한 비밀번호 뿐 아니라 동일한 숫자나 문자, common한 비밀번호 (`asdfasdf`) 등을 사용하지 못하게 해준다. (`is_password_usable()`로 확인 가능)

이런 강력한 장고 비밀번호 관리는 [해당 문서](https://docs.djangoproject.com/en/2.0/topics/auth/passwords/#module-django.contrib.auth.password_validation)에서 더 자세하게 확인할 수 있다.


하지만 이런 기능이 가끔 불편할 때도 있다. 간단한 사이트의 user/password에 장고의 기본 validation을 사용하지 않고 싶을 때가 있는데, 이럴땐 장고 settings의 `AUTH_PASSWORD_VALIDATORS`를 바꿔주면 된다.

기본으로는 다음과 같이 있다.
```python
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
        'OPTIONS': {
            'min_length': 9,
        }
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]
```

각각의 설명은 다음과 같다.

- `UserAttributeSimilarityValidator`: username (id나 email)과 비밀번호가 유사한지 체크
- `MinimumLengthValidator`: default 8자와 다른 최소 비밀번호 숫자를 지정할 수 있다
- `CommonPasswordValidator`:  common한 비밀번호와 체크. 기본으로 1000개의 common 비밀번호 리스트와 비교한다.
- `NumericPasswordValidator`: 전부 숫자인 비밀번호를 사용하지 못하게 한다.

이 네개 중에 필요한 것들은 사용하고, 필요하지 않은 것은 제외하면 된다.



이뿐만 아니라 장고에서 제공하는 위의 4가지 validator 말고 자신의 custom validator를 작성할 수도 있다.

물론 `signup` view form, `user` model 등에서 비밀번호 폼을 확인해도 되지만 이런 validator를 사용하는 것이 더 파이써닉하고 좋은 방법인 것 같다. 다음에 기회가 있다면 꼭 사용해봐야지.
```python
from django.core.exceptions import ValidationError
from django.utils.translation import gettext as _

class MinimumLengthValidator:
    def __init__(self, min_length=8):
        self.min_length = min_length

    def validate(self, password, user=None):
        if len(password) < self.min_length:
            raise ValidationError(
                _("This password must contain at least %(min_length)d characters."),
                code='password_too_short',
                params={'min_length': self.min_length},
            )

    def get_help_text(self):
        return _(
            "Your password must contain at least %(min_length)d characters."
            % {'min_length': self.min_length}
        )
```
