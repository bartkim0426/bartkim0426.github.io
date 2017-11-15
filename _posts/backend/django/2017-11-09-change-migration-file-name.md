---
layout: post
title:  "Change django migration file name after migrate"
categories: django migrate
---

django에서 migration 파일을 관리하다 보면, 서로 다른 곳에서 `makemigration` 명령어를 진행해 서로 같은 내용의 migration 파일이 이름만 다른 경우가 발생한다. (물론 한 곳에서만 makemigration 하는 것이 가장 좋은 방법인것 같다.)

이런 경우에 git을 통해 파일 명을 맞춰버리면 migrate 을 했을 경우에 이름이 다른 파일을 전혀 다른 migration 파일로 인식하기 때문에 동일한 내용을 두 번 migrate하여 대개 `already exist` 에러가 발생한다.

다양한 방법으로 해결이 가능하지만, 가장 편리한 방법은 역시 `--fake` migrate 방법이다.


## 1. 파일명 변경
- 이름이 다른 파일의 파일명 (대개는 시간이 다른 경우가 많다)을 변경해준다. (안에 내용에도 주석 처리된 시간이 있기 때문에 이를 처리해 주는 것이 좋다)

## 2. dependencies 설정
- (만약) 이미 그 이후로 다른 migration 파일이 생겨 해당 파일이 dependency로 들어가 있다면 해당 migration 파일들의 이름을 모두 수정해준다.

## 3. `--fake` migrate 실행
```
python manage.py migrate --fake
```
명령어를 통해 해당 migration 파일을 fake migrate 시킨다. 


이후 생성된 migration 파일들에 대해서는 문제 없이 잘 작동한다.
