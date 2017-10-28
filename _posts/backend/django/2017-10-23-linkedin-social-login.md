---
layout: post
title:  "Linkedin social login with django allauth"
categories: django
tag: django social allauth
---

> [이전 포스트]의 allauth 사용법을 읽고 allauth 설치하고, INSTALLED_APP에 linked-in을 추가를 완료한 후 이 포스트를 따라해야함


### 1. 앱 만들기
[linkedin developer](https://www.linkedin.com/developer/apps)로 들어가서 새로운 앱을 만든다.

회사명, 이름, 사용할 URL 등을 적은 후 만들어진 앱의 클라이언트 ID, 비밀 키를 django admin의 social application에 추가해준다. 

### 2. OAuth 2.0 에 인증된 리다이렉트 URL 추가하기

redirect url로 
`http://localhost:8000/accounts/linkedin_oauth2/login/callback/`를 추가

### 3. settings

```
SOCIALACCOUNT_PROVIDERS = {
    'linkedin': {
        'SCOPE': [
            'r_emailaddress',
        ],
        'PROFILE_FIELDS': [
            'id',
            'first-name',
            'last-name',
            'email-address',
            'picture-url',
            'public-profile-url',
        ]
    }
}
```


### 4. 설정에서 상용화

