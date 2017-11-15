---
layout: post
title: "AWS cli 설치 및 s3 업로드, 다운로드하기"
categories: aws
tag: aws cli s3
---

aws 서비스를 사용해 보면서 느낀 가장 큰 장점 중 하나는 직관적이고 매우 간편한 gui(콘솔)이다. 서버 안에서 복잡하게 해야 할 것들을 콘솔에서 쉽게 클릭 몇 번으로  끝낼 수 있다는 것이다. 

다만 ec2와 연계해서 사용하다 보면 ec2의 파일들을 s3에 올린다던지 등 gui를 사용하기가 애매한 상황들이 있기 때문에 terminal 내에서 aws와 연동해서 사용할 수 있는 aws cli의 사용법을 기초적으로나마 알고 있어야 할 것 같다.

## aws cli 설치
[aws cli github document](https://github.com/aws/aws-cli)나 [aws official document(한글)](http://docs.aws.amazon.com/ko_kr/cli/latest/userguide/installing.html)을 보고 따라하면 아주 간단하게 설치 가능하다.

pip나 apt 중 원하는 걸로 설치 가능하다. virtualenv가 아닌 전역에서 사용하려면 apt를 이용하여 설치하면 된다.

### 1. pip를 사용하여 설치
```
pip install awscli
```

### 2. apt를 사용하여 설치
```
sudo apt install awscli
```

## aws cli 기본 설정
cli를 설치했다면 aws access key, secret key 등을 설정 해 주어야 한다. 해당 키는 `IAM`에서 만들 수 있다. 

```
aws configure
```
위 명령어를 실행하면 aws에 대해 설정할 내용들이 쭉 나온다.
```
AWS Access Key ID [None]: 
# IAM의 access key를 넣어준다.

AWS Secret Access Key [None]:  
# IAM의 Secret Key를 넣어준다.

Default region name [None]:  
# default로 cli를 사용할 리젼을 넣어준다. seoul region을 사용할 경우 
# ap-northeast-2 를 넣어준다.

Default output format [None]:
# 그냥 엔터
```

## aws cli로 s3 명령어 사용
aws cli를 설치 했으면, s3에 acess하여 파일을 올리고 삭제하는 등을 ec2 내에서 쉽게 할 수 있다. 

명령어는 linux 명령어와 거의 흡사하여 기존에 linux를 조금 다룰 줄 아는 사람이라면 쉽게(?) 사용 할 수 있다.

`aws s3 help`로 각종 명령어들을 확인할 수 있지만, 자주 사용하는 명령어들만 정리해놓았다.

```
# 현재 s3 버킷의 list
aws s3 ls

# s3 버킷에서 지우기
aws s3 rm

# bucket 생성하기, 삭제하기
aws s3 mb
aws s3 rb

# ec2에서 s3로 옮기기
aws s3 cp
aws s3 mv

# ec2, bucket 폴더 동기화하기
aws s3 sync
```
