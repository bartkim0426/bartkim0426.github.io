---
layout: post 
title:  "serverless 사용해보기"
categories: server
---

[serverless github](https://github.com/serverless/serverless)

[medium blog](https://medium.com/@jwyeom63/%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EB%8A%94-node-js%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4-serverless-503ee61539d://medium.com/@jwyeom63/%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EB%8A%94-node-js%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4-serverless-503ee61539d4)

## 1. 설치하기

```bash
npm install -g serverless
```

## 2. AWS 세팅

- IAM으로 유저 생성
- AdministratorAccess 권한 부여
- key / secret key 발급
- 발급한 키를 serverless setting에 추가

```bash
serverless config credentials --provider aws --key xxxxxxxxxxxxxx --secret xxxxxxxxxxxxxx
# 기존에 aws credeital을 사용중이라면 -o 로 override 가능
# 만약 aws credential에 다른 프로필을 사용중이라면 --profile로 지정 가능
```

## 3. 서비스 만들기

기존의 템플릿으로 쉽게 만들 수 있다.

```bash
serverless create --template aws-python3 --path my-service
# serverless create -h 로 템플릿 목록 확인 가능
```

## 4. 서비스 수정

생성된

```
# serverless.yml 
service: my-service
provider:   
  name: aws   
  runtime: python3.7
functions:
  hello:
    handler: handler.hello
    events: # uncomment these lines
      - http:
          path: hello/get
          method: get
```

## 5. 배포

```bash
serverless deploy -v
# 테스트는 다음 명령어로 실행 가능
# serverless invoke -f hello -l
```

## 6. local serverless 구축

- [serverless-offline](https://github.com/dherault/serverless-offline)을 활용
- 로컬에서 작업 후 배포 가능

### 설치

```bash
npm init
npm install serverless-offline --save-dev
```

### plugins 추가

```
# serverless.yml
service: serverless-example # NOTE: update this with your service name

provider:
  name: aws
  runtime: python3.7

functions:
  hello:
  handler: handler.hello
  events:
    - http:
      path: users/create
      method: get

plugins:
  - serverless-offline
```

### offline 실행

```bash
serverless offline start
```

## 7. 모니터링

[dashbird](https://dashbird.io/) 서비스 이용 가능

