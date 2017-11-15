---
layout: post
title: "AWS IAM으로 사용자 만들기"
categories: aws iam
---

AWS를 사용하다 보면 s3 등 사용자 권한이 필요한 경우가 있다. 이런 경우 iam을 통해 유저를 만들고 `aws access key`, `secret key`를 발급 받아야 한다. 


## IAM 유저 생성

### user 만들기   


aws 콘솔에서 `IAM` 서비스로 이동한다. 기존에 User가 없다면 user를 만들어야 한다. 
user 창으로 들어가 `Add user`를 한 뒤 username을 입력해 주고, access type을 지정해 준다.
 AWS 콘솔용 유저라면 `AWS management console access`를, cli를 사용하려면 `Programmatic access`에 체크를 해 준다.

### (group이 없다면) group 생성해주기   


user를 만들 때 해당 유저의 그룹을 생성 해 주어야 하는데, 처음 만드는 경우라면 그룹이 없을 테니 그룹 또한 생성해준다. 이 그룹은 해당 유저들이 어떤 권한을 갖는지를 지정해준다.
group name을 적어주고, `policy type`을 선택해야 한다. policy type은 해당 유저들이 어떤 역할을 하는지를 부여하는 것인데, s3를 사용할 것이라면 s3를 검색해서 `AmazonS3FullAcess`에 체크를 해 주면 된다.
 user는 그룹별로 여러 policy를 지정하여 사용해도 되고, 한 그룹당 한 policy를 지정해서 멀티로 선택해서 사용해도 된다. 

### aws access key, secret key 생성   


유저를 생성하면 아래 access key, secret key가 생성된 것을 볼 수 있다. 화면을 닫아버리면 다시 볼 수 없으니 show로 secret key를 확인 한 후 자신만 볼 수 있게 간직해둬야한다.
