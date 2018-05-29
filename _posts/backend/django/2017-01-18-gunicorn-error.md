---
layout: post
title:  "django gunicorn error: no module named 0"
categories: django
---

gunicorn 명령어(`gunicorn 0.0.0.0:8000 config.wsgi:applicatoin`) 실행시 wsgi 파일의 모듈을 못 찾는 에러 발생.

```bash
Traceback (most recent call last):
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/arbiter.py", line 578, in spawn_worker
    worker.init_process()
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/workers/base.py", line 126, in init_process
    self.load_wsgi()
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/workers/base.py", line 135, in load_wsgi
    self.wsgi = self.app.wsgi()
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/app/base.py", line 67, in wsgi
    self.callable = self.load()
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/app/wsgiapp.py", line 65, in load
    return self.load_wsgiapp()
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/app/wsgiapp.py", line 52, in load_wsgiapp
    return util.import_app(self.app_uri)
  File "/Users/seulchankim/.pyenv/versions/3.6.0/envs/sayno/lib/python3.6/site-packages/gunicorn/util.py", line 352, in import_app
    __import__(module)
ModuleNotFoundError: No module named '0'
```

찾아보니 서브디렉토리에 `wsgi` 파일이 있는 경우 `wsgi` 파일 안의 `os.environ.setdefault("DJANGO_SETTINGS_MODULE")`을 실행시키지 못해서 발생하는 문제.

실제로 `wsgi.py`가 있는 디렉토리에서 작업을 해보면 정상적으로 작동되는 것을 볼 수 있다.
```bash
$ cd .. && gunicorn --bind 0.0.0.0:8000 config.wsgi:application
```

그래서 찾은 해결책은 `PYTHONPATH`를 이용해서 사용하는 방식. 해당 path를 지정해 준 이후에 `gunicorn` 명령어를 실행하면 된다.

```bash
PYTHONPATH=`pwd`/.. gunicorn --bind 0.0.0.0:8000 config.wsgi:application
```


출처: [SO 답변](https://stackoverflow.com/questions/39460892/gunicorn-no-module-named-myproject)
