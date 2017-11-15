---
layout: post
title:  "django get site-url in template"
categories: django tips
---
project를 진행시 한 코드를 여러 url에 물리고 싶을 때가 있다. 그럴 때 javascript 내에서 절대경로로 하드코딩하게 되면 추후에 모든 url을 다 수정해야 하는 일이 발생하기 때문에 모든 url domain을 물리는 방법을 찾아보았다.


## `request.META.HTTP_HOST` 사용하기
`request.META`가 가지고 있는 여러 정보들 중 `HTTP_HOST`를 사용하면 현재 request가 접속중인 HOST, 즉 절대경로를 던져준다. 이걸 써도 무방하지만 request를 받아야만 해당 site url을 반환하기 때문에 다른 방법을 사용하였따.


## Template context_processor 사용하기
> 만약 settings.py를 local, production, aws 등 나누어 관리한다면 다음 방법이 편리하다.



### local, production 등 각각의 setting 파일에 `SITE_URL`을 추가해준다.

```
# in local.py settings
SITE_URL = "http://localhost:8000/"

# in production.py
SITE_URL = "http://your_domain.com/"

# in other.py
SITE_URL = "http://other_domain.com/"
```

### 다음으로 SITE_URL을 context로 넘기는 `get_site` 모듈을 만들어준다. 
```
# in common app
from django.conf import settings

def get_site(request):
    return {"SITE_URL": settings.SITE_URL}
```

### `settings.py`의 TEMPLATE 부분의 context_processor에 만들어 놓은 `get_site` 모듈을 추가한다.
```
# in settings.py
# 나는 setting을 잘게 쪼개어 TEMPLATE.py에 다음 내용을 넣어 두었다.
...
TEMPLATES = [
    {
		...
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
				# 다음과 같이 'app_name.module' 로 만들어 놓은 get_site를 추가
                'common.get_site',
				# 나는 utils라는 모듈 디렉토리 안에서 만들었기 때문에 다음과 같이 추가하였다.
				# 'common.utils.get_site',
            ],
            'loaders': [
                'django.template.loaders.filesystem.Loader',
                'django.template.loaders.app_directories.Loader',
            ],
        },
		...
    },
]
...
```

### template 내에서 호출하기
다음과 같이 템플릿 태그를 사용해서 만들어 놓은 SITE_URL을 쉽게 호출 가능하다.

```
# in template
{{ SITE_URL }}
```


## 정리
template의 context_processors는 한 번도 생성해 본 적이 없었는데 SITE_URL을 넣으면서 굉장히 쉽게 구현할 수 있었다. template 전역에서 뿌리고 싶은 데이터가 있으면 model, view 말고 context_processors를 사용해서 뿌려주면 훨씬 쉽고 간편하게 사용 가능할 듯 하다.
