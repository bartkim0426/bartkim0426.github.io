---
layout: post
title:  "Django: show invalid message in django loginform"
categories: django
---


## Inherit `AuthenticationForm`

Inherit `django.contrib.auth.forms.AuthenticationForm` and customize `clean()` method.

```python
from django.contrib.auth.forms import AuthenticationForm
from django.contrib.auth import authenticate
from django import forms

from my_user.models import MyUser


class CustomLoginForm(AuthenticationForm):

    error_messages = {
        'invalid_login':
            "아이디 또는 비밀번호를 다시 확인하세요.\n등록되지 않은 아이디이거나, 아이디 또는 비밀번호를 잘못 입력하셨습니다."
        ,
        'inactive': "승인 대기중인 계정입니다.\n관리자 승인 이후에 문자 전송드리겠습니다.",
    }

    def clean(self):
        username = self.cleaned_data.get('username')
        password = self.cleaned_data.get('password')
        if username is not None and password:
            self.user_cache = authenticate(self.request, username=username, password=password)
            if self.user_cache is None:
                try:
                    user_temp = MyUser.objects.get(username=username)
                except:
                    user_temp = None

                if user_temp is not None and user_temp.check_password(password):
                    self.confirm_login_allowed(user_temp)
                else:
                    raise forms.ValidationError(
                        self.error_messages['invalid_login'],
                        code='invalid_login',
                        params={'username': self.username_field.verbose_name},
                    )

        return self.cleaned_data
```

## Using custom form in loginview

```python
from django.contrib.auth import (
    authenticate,
    login
)
from django.http import HttpResponseRedirect

from django.contrib.auth.views import LoginView
from mobile_template.views import MobileTemplateView
from ..forms import CustomLoginForm


class LoginAccessView(MobileTemplateView, LoginView):
    authentication_form = CustomLoginForm
    template_name = 'registration/login.html'
```
