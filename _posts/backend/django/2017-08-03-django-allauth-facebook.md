---
layout: post
title:  "django allauth facebook login"
categories: django
tags: django
---

## django facebook login with allauth

### useful sites
-[django allauth document](http://django-allauth.readthedocs.io/en/latest/providers.html)

### basic usage
- install allauth  
`pip install django-allauth`
- edit `settings.py`   


```python
# Specify the context processors as follows:
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                # Already defined Django-related contexts here

                # `allauth` needs this from django
                'django.template.context_processors.request',
            ],
        },
    },
]

AUTHENTICATION_BACKENDS = (
    ...
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
    ...
)
INSTALLED_APPS = (
    ...
    # The following apps are required:
    'django.contrib.auth',
    'django.contrib.sites',

    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'allauth.socialaccount.providers.facebook',
	...
)
```
- edit `urls.py`
```python
urlpatterns = [
    ...
    url(r'^accounts/', include('allauth.urls')),
    ...
]
```

- add site for domain in admin
1. add my domain (in dev, just add `localhost:8000/`) in `localhost/admin/sites`
2. add socialapp: `/socialaccount/socialapp/` with api/secret key, site


### Add facebook settings in `settings.py`

```python
# Some really nice defaults
ACCOUNT_AUTHENTICATION_METHOD = 'username'
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'none'

ACCOUNT_ALLOW_REGISTRATION = True
# ACCOUNT_ADAPTER = 'artsumer.users.adapters.AccountAdapter'
# SOCIALACCOUNT_ADAPTER = 'artsumer.users.adapters.SocialAccountAdapter'

SOCIALACCOUNT_PROVIDERS = {
    'facebook': {
        'METHOD': 'oauth2',
        'SCOPE': ['email', 'public_profile', 'user_friends'],
        'AUTH_PARAMS': {'auth_type': 'reauthenticate'},
        'FIELDS': [
            'id',
            'email',
            'name',
            'first_name',
            'last_name',
            'verified',
            'locale',
            'timezone',
            'link',
            'gender',
            'updated_time'],
        'EXCHANGE_TOKEN': True,
        'LOCALE_FUNC': lambda request: 'kr_KR',
        'VERIFIED_EMAIL': False,
        'VERSION': 'v2.8'
    }
}

LOGIN_REDIRECT_URL = "/"

# SITE_ID with your domain site in /admin/sites/
SITE_ID = 6

AUTH_USER_MODEL = 'artsumersuser.ArtsumersUser'
```
