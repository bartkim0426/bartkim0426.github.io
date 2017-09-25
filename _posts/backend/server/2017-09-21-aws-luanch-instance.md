---
layout: post
title:  "Launch AWS instance"
categories: server aws
---

## Launch Aws instance

1. make instance in [aws console](https://ap-northeast-2.console.aws.amazon.com/ec2/v2/)

2. make secret key
저장 후 특정 폴더에 보관한다. 나의 경우는
`~/.ssh/security/` 하위의 프로젝트명 아래 보관한다.


3. change auth of secret key file
`chmod 400 my-key-pair.pem`

4. access to aws server
```
# Put your aws server
ssh -i ~/path/to/my-key-pair.pem ubuntu@ec2-52-79-127-105.ap-northeast-2.compute.amazonaws.com
```

5. (optional) Make alias
매번 명령어를 치고 들어가기 귀찮으니, `~/.zshrc`(혹은 `~/.bashrc`, `~/.bashprofile`)에 alias를 설정해준다.
```
# open zshrc
vim ~/.zshrc

# make alias for aws instance
# aliasname에 원하는 alias 명을 입력
alias aliasname='ssh -i ~/.ssh/security/my-key-pair.pem ubuntu@ec2-52-79-127-105.ap-northeast-2.compute.amazonaws.com'
```
