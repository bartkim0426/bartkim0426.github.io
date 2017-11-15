---
layout: post
title:  "django project에 cloudfront(s3) 연결하기"
categories: django aws s3
---

## 0. aws s3 생성 및 cloudfront 연동
이 부분은 permission, property 등 생각보다 설정해야 할 것이 많으므로 [다른 포스팅]()에서 따로 다룰 예정이다.

## 1. 초기 라이브러리 설치

### 1. Install django-storage
- [공식문서](https://django-storages.readthedocs.io/en/latest/)를 참고해서 `django-storage` 를 설치
- 설치 이후에는 `settings.py`에 추가해준다.


```
INSTALLED_APPS = (
    ...
    'storages',
    ...
)
```

### 2. Install boto3
- [boto 공식 문서](http://boto3.readthedocs.io/en/latest/)를 참고하여 설치 (`pip install boto3`)


## 2. `MediaStorage` module 추가하기 
`S3Boto3Storage`를 상속받는 `MediaStorage`를 만들어야한다. 위치는 settings.py가 있는 디렉토리나, common app(utils, mixin 등을 넣어놓는 앱을 만들어 놓았다면)에 넣으면 된다. 

나는 mixin, utils, 일반 index page view 등을 가지고 있는 common 앱의 utils 아래에 만들었따.
```
from django.conf import settings

from storages.backends.s3boto3 import S3Boto3Storage

# if use media storage
class MediaStorage(S3Boto3Storage):
	# 만약 settings에서 MEDIAFILES_LOCATION을 불러오려면 다음과 같이 설정 가능
    # locaion = settings.MEDIAFILES_LOCATION
	# bucket에 미디어 파일을 넣는 directory name
	location = "mediafiles"

    def __init__(self, *args, **kwargs):
        kwargs['custom_domain'] = settings.AWS_CLOUDFRONT_DOMAIN
        super(MediaStorage, self).__init__(*args, **kwargs)

# if use static storage
class StaticStorage(S3Boto3Storage):
	# bucket에 static 파일을 넣는 directory name
	location = "staticfiles"

    def __init__(self, *args, **kwargs):
        kwargs['custom_domain'] = settings.AWS_CLOUDFRONT_DOMAIN
        super(StaticStorage, self).__init__(*args, **kwargs)
```


## 3. django setting하기


### 1. aws IAM에서 발급받은 aws key 추가해주기
- aws IAM에서 발급받은 key, secret key를 해당 프로젝트에 추가해준다. 
> 참고: [aws IAM에서 key, secret key 발급받기]()
- `settings.py`에 바로 넣어도 무방(?) 하지만, `github`등으로 프로젝트를 관리하는 경우나 보안상 안전을 위해서 따로 관리하는 것이 좋다!
- `.env`를 사용해도 되고, 서버인 경우에는 `~/.bashrc`에 추가해주어도 된다. 
- 다음은 `.env`에 추가해서 불러오는 예시이다

`.env` 파일에서 다음과 같이 설정해준다.
```
...
# YOUR_ACCESS_KEY_ID, YOUR_SECRET_ACCESS_KEY 대신에 자신의 key, secret key를 넣어주어야한다.
DJANGO_AWS_ACCESS_KEY_ID='YOUR_ACCESS_KEY_ID'
DJANGO_AWS_SECRET_ACCESS_KEY='YOUR_SECRET_ACCESS_KEY'
...
```

이후 `.env`의 값을 `settings.py`에서 불러와준다.
```
...
# aws access_key
AWS_ACCESS_KEY_ID = os.environ["DJANGO_AWS_ACCESS_KEY_ID"]
AWS_SECRET_ACCESS_KEY = os.environ["DJANGO_AWS_SECRET_ACCESS_KEY"]

# aws s3 bucket name
AWS_STORAGE_BUCKET_NAME = 'dcap.community'
...
```

### 2. AWS  세팅 추가하기
`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` 이외의 s3 세팅을 마무리해준다

```
...
AWS_ACCESS_KEY_ID = os.environ["DJANGO_AWS_ACCESS_KEY_ID"]
AWS_SECRET_ACCESS_KEY = os.environ["DJANGO_AWS_SECRET_ACCESS_KEY"]

# cloudfront domain to add MediaStorage
# rout53에서 연결한 cloudfront domain을 적거나 cloudfront url을 적어준다.
AWS_CLOUDFRONT_DOMAIN = 'd1xzxxxxxxxx.cloudfront.net'
AWS_CLOUDFRONT_DOMAIN = 'media.mydomain.com'

# default storage for access cloudfront
DEFAULT_FILE_STORAGE = 'gcfapp.common.utils.MediaStorage'
```

### 3. Django https 세팅 추가
```
...
# for django https config
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
...
```
