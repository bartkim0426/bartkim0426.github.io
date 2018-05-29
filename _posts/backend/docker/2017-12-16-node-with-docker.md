---
layout: post 
title:  "build node image in docker"
categories: docker
tags: docker node
---

udemy 강의 예시 (docker로 node 서버 image 만들어서 띄우기)


> 배포할 node가 이미 완성되어 있다고 가정하고 진행


### `Dockerfile` 만들기

```
# Node official image 가져오기 (6-alpine 버전)
FROM node:6-alpine

# 3000port listen
EXPOSE 3000

# package manager로 tini 설치
RUN apk add --update tini

# 'mkdir -p /usr/src/app' 코맨드
RUN mkdir -p /usr/src/app

# - Node uses a "package manager", so it needs to copy in package.json file
# package manager 사용을 위한 'package.json' 파일 이동
# package.json 파일은 /usr/src/app 안에 위치해야 하므로 해당 dir로 이동
WORKDIR /usr/src/app
COPY package.json package.json

# npm install과 npm cache clean을 함께 하는게 좋음
# '&&'는 두 명령어 관계 없이 실행. '&'은 앞의 명령어가 성공해야지만 다음 명령어를 실행
RUN npm install && npm cache

# - then it needs to copy in all files from current directory
# 현재 directory의 모든 파일 복사할때는 이 명령어 사용
COPY . .

# cmd command로 'tini -- node ./bin/www' 실행. json 형태로 스트링을 콤마(,)로 구분
CMD ["tini", "--", "node", "./bin/www"]
```

### build dockerfile 
```
$docker image build -t testnode .
```

### run container
```
$docker container run --rm -p 80:3000 testnode
```
> `localhost`에 접속하면 node server가 정상적으로 작동하는것을 볼 수 있다.

### image push하고 테스트해보기
이미지가 정상 작동하는것을 확인하였으면 이를 hub에 push한 뒤 local의 image를 지우고 hub의 이미지로 테스트해본다.
```
# tag 달기
$docker image tag testnode bartkim0426/testing-node

# push
$docker image push bartkim0426/testing-node

# delete local image
$docker image rm testnode
$docker image rm bartkim0426/testing-node

# hub에서 이미지 받아와서 작동하는지 확인
$docker container run -p 80:3000 --rm bartkim0426/testing-node
```
