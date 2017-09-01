---
layout: post
title:  "pytest django"
categories: django pytest
tags: django
---

> django에 TDD를 적용하기에 앞서 pycon 2017에서 세션을 들은 pytest를 사용해보기로 하였다. pytest를 찾아보던 중, [Why I use py.test and you probably should too](https://cjh5414.github.io/why-pytest/) 라는 좋은 글을 번역해 놓은 것이 있어서 읽으면 큰 도움이 되겠다.

# Using django-pytest

## Install pytest
`$ pip install pytest-django`

## Setting
테스트를 실행할 루트 디렉토리 (`manage.py`가 위치한 디렉토리)에 pytest.ini을 생성한다. 그 후 테스트에 사용할 설정 파일을 지정해준다. 실행시 --settings로 따로 지정해도 괜찮다. 이 외 다양한 설정은 [pytest 문서](https://docs.pytest.org/en/latest/)를 확인.

```
[pytest]
DJANGO_SETTINGS_MODULE=myapp.settings
# 혹시 test를 못 찾는다면 다음 설정을 추가, recommended
python_files = tests.py test_*.py *_tests.py
```

## Usage
`$ pytest` 로 쉽게 테스트를 구동 가능하다.
