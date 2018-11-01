---
layout: post 
title:  "aws에 새로운 user 생성 이후 public key 추가하기"
categories: server
---


웹 서버를 띄울 때 보안과 여러가지 이유를 위해 새로운 user를 사용하는게 좋다고 하여 이번 aws를 사용할 때는 기본 `ubuntu`를 쓰지 않고 새로운 user를 만들어서 사용해 보았다.

`keygen` 형태의 보안에 익숙하지 않고, user 명령어 등도 낯설어서 조금 헤메었다. 시행착오를 겪지 않고자 기록해둔다.



### 새로운 user 생성

서버 상황에 따라 `sudo`권한으로 실행시켜야 할 수도 있다.

```
# aws에서 발급받은 pem키로 로그인 할 것이기 때문에 password 없이 new_user를 생성
adduser new_user --disabled-password

# sudo 권한 부여
usermod -aG sudo new_user

# new_user로 로그인
sudo su new_user
```

### ssh key 확인하기
같은 서버의 root (ubuntu) user의 `authorized_keys`에서 확인해도 되고, `pem`(private key)를 가지고 있는 클라이언트측에서 확인해도 된다. 

여기서는 클라이언트 pc (서버로 접속하려는 pem 키가 있는 pc)에서 확인하는 방법을 설명하겠다

```
# 다음 명령어를 작성하면 pem키 경로를 작성하라는 프롬프트창이 나온다.
# 해당 pem키의 경로를 작성해주면 ssh public key가 나온다.
ssh-keygen -y
Enter file in which the key is (/Users/username/.ssh/id_rsa): /example/path/to/example.pem
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClKsfkNkuSevGj3eYhCe53pcjqP3maAhDFcvBS7O6V
hz2ItxCih+PnDSUaw+WNQn/mZphTk/a/gU8jEzoOWbkM4yxyb/wB96xbiFveSFJuOp/d6RJhJOI0iBXr
lsLnBItntckiJ7FbtxJMXLvvwJryDUilBMTjYtwB+QhYXUMOzce5Pjz5/i8SeJtjnV3iAoG/cQk+0FzZ
qaeJAAHco+CY/5WrUBkrHmFJr6HcXkvJdWPkYQS3xqC0+FmUZofz221CBt5IMucxXPkX4rWi+z7wB3Rb
BQoQzd8v7yeb7OzlPnWOyN0qFU0XA246RA8QFYiCNYwI3f05p6KLxEXAMPLE
```

이제 해당 public key를 복사하여 서버에 추가해보자.

### ssh key 추가
해당 user는 ssh public key를 가지고 있지 않은 상태이기 때문에 private key가 있어도 로그인이 불가능하다. 따라서 같은 public key를 추가해주어야 한다.

```
cd
mkdir .ssh
chmod 700 .ssh

cd
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys

# 이제 만들어진 파일에 이전 public key를 추가해주어야 한다.
# 아까 복사해둔 public key를 붙여넣기 해준다.
vim .ssh/authorized_keys
```


이제 새로 만들어진 user로 로그인이 가능하다.

```
ssh -i ~/.ssh/path/to/pem new_user@your_aws_server
```


### 기타사항
계속 비밀번호를 입력하라고 뜬다면 `passwd new_user`로 비밀번호를 지정해주면 된다.
