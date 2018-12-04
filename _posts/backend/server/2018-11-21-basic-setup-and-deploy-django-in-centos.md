---
layout: post 
title:  "centos에 django setup"
categories: server
---


지금까지는 ubuntu에 주로 셋업을 했었기 때문에 centos에 setup을 한 기록을 남겨눔


## Add user

```
adduser user_name
passwd user_name

# give wheel group to add sudo privileges
usermod -aG wheel user_name

# test login
su - user_name
```

## Install python 3 in centos 6

```
sudo yum install epel-release

curl 'https://setup.ius.io/' -o setup-ius.sh

# 이제 python3 접근 가능
sh setup-ius.sh
# 버전 확인
yum --enablerepo=ius info python36u
# 설치
sudo yum --enablerepo=ius install python36u

# python 사용 가능
python3.6
>> python 3.6.5
```

## Install virtualenv
- https://www.vultr.com/docs/how-to-install-and-use-pip-and-virtualenv-on-centos-6 참고
- https://gist.github.com/hangtwenty/5546945 참고

```
sudo yum update
# install pip3
wget https://bootstrap.pypa.io/get-pip.py
python3.6 get-pip.py
sudo yum -y install python36u-pip

# install virtualenv
pip3 install virtualenv
pip3 install virtualenvwrapper
echo 'export WORKON_HOME=~/Envs' >> .bashrc # Change this directory if you don't like it
source $HOME/.bashrc
mkdir -p $WORKON_HOME
echo '. /usr/bin/virtualenvwrapper.sh' >> .bashrc
source $HOME/.bashrc
```

안되면 bashrc에 다음 추가
```
if [ "$VIRTUALENVWRAPPER_PYTHON" = ""  ] 
then
    VIRTUALENVWRAPPER_PYTHON="$(command \which python3.6)"
fi
```

```
mkvirtualenv test
workon test
```





## Install postgres

```
vim nano /etc/yum.repos.d/CentOS-Base.repo
```

base, updates 마지막줄에 `exclude=postgresql*` 추가

```
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
exclude=postgresql*

[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
exclude=postgresql*
```

postgres 다운로드 사이트 ([여기](https://yum.postgresql.org/repopackages.php))에 가서 해당 버전에 맞는 다운로드 링크를 복사,

```
cd ~
sudo curl -O https://download.postgresql.org/pub/repos/yum/10/redhat/rhel-6-x86_64/pgdg-centos10-10-2.noarch.rpm

sudo rpm -ivh pgdg*
```


postgres 확인하기

```
yum list postgres*

Installed Packages
postgresql.x86_64                                                     8.4.20-4.el6_7                                           @updates
postgresql-devel.x86_64                                               8.4.20-4.el6_7                                           @updates
postgresql-libs.x86_64                                                8.4.20-4.el6_7                                           @updates
Available Packages
postgresql-ip4r.x86_64                                                1.05-1.el6                                               epel
postgresql-jdbc.noarch                                                42.2.5-1.rhel6                                           pgdg10
postgresql-jdbc-javadoc.noarch                                        42.2.5-1.rhel6                                           pgdg10
postgresql-pgpool-II.i686                                             3.2.15-1.el6                                             epel
postgresql-pgpool-II.x86_64                                           3.2.15-1.el6                                             epel
postgresql-pgpool-II-devel.i686                                       3.2.15-1.el6                                             epel
postgresql-pgpool-II-devel.x86_64                                     3.2.15-1.el6                                             epel
postgresql-pgpool-II-recovery.x86_64                                  3.2.15-1.el6                                             epel
postgresql-plruby.x86_64                                              0.5.3-4.el6                                              epel
postgresql-plruby-doc.x86_64                                          0.5.3-4.el6                                              epel
postgresql10.x86_64                                                   10.6-1PGDG.rhel6                                         pgdg10
postgresql10-contrib.x86_64                                           10.6-1PGDG.rhel6                                         pgdg10
postgresql10-debuginfo.x86_64                                         10.6-1PGDG.rhel6                                         pgdg10
postgresql10-devel.x86_64                                             10.6-1PGDG.rhel6                                         pgdg10
postgresql10-docs.x86_64                                              10.6-1PGDG.rhel6                                         pgdg10
postgresql10-libs.x86_64                                              10.6-1PGDG.rhel6                                         pgdg10
postgresql10-odbc.x86_64                                              10.03.0000-1PGDG.rhel6                                   pgdg10
postgresql10-plperl.x86_64                                            10.6-1PGDG.rhel6                                         pgdg10
postgresql10-plpython.x86_64                                          10.6-1PGDG.rhel6                                         pgdg10
postgresql10-pltcl.x86_64                                             10.6-1PGDG.rhel6                                         pgdg10
postgresql10-server.x86_64                                            10.6-1PGDG.rhel6                                         pgdg10
postgresql10-tcl.x86_64                                               2.4.0-1.rhel6                                            pgdg10
postgresql10-tcl-debuginfo.x86_64                                     2.3.1-1.rhel6                                            pgdg10
postgresql10-test.x86_64                                              10.6-1PGDG.rhel6                                         pgdg10
postgresql_anonymizer10.noarch                                        0.2.1-1.rhel6                                            pgdg10
postgresql_autodoc.noarch                                             1.41-1.el6                                               epel
```

위에 나온 버전(10)에 맞게 설치

```
sudo yum install postgresql10-server
```

설치 이후 db init, service 키기

```
service postgresql-10 initdb
chkconfig postgresql-10 on
service postgresql-10 start
```

postgres user로 로그인 이후 새로운 계정 생성 (option)


```
sudo su - postgres
psql
postgres=# CREATE ROLE role_name;
postgres=# ALTER ROLE role_name WITH PASSWORD 'password';
# 원하는 ROLE 부여 
postgres=# ALTER ROLE role_name WITH login createdb superuser;
postgres=# CREATE DATABASE crowdbase TEMPLATE template0 LC_COLLATE 'C';
postgres=# GRANT ALL PRIVILEGES ON DATABASE db_name TO role_name;
```


## sudoers에 wheel 그룹 추가하기 (optinoal)

```
chmod u+x /etc/sudoers
vim /etc/sudoers
```
 
주석 해제. `wheel` 그룹에 수도권한 부여
```
%wheel        ALL=(ALL)       NOPASSWD: ALL
```

이후 다시 `440`으로 변경해야만 오류가 나지 않음
```
chmod 440 /etc/sudoers

# test
sudo tail /etc/sudoers
```

## Upgrade git (optinoal)
git version이 `1.7` 이하면 `https` 깃헙 저장소를 받는데 문제가 발생할수도 있음.

```
fatal: HTTP request failed
```

새로운 git을 설치해주면 된다

```bash
sudo rpm -Uvh http://opensource.wandisco.com/centos/6/git/x86_64/wandisco-git-release-6-1.noarch.rpm

# git 검색
yum --enablerepo=WANdisco-git --disablerepo=base,updates info git

# git 설치
yum --enablerepo=WANdisco-git --disablerepo=base,updates install git
```

## Install tmux (optinal)
다음 쉘스크립트를 받은 이후  실행

(https://gist.github.com/bartkim0426/84da37a95be96185099d46b4fb390419)

```
wget https://gist.github.com/bartkim0426/84da37a95be96185099d46b4fb390419
chmod +w install-tmux.sh
./install-tmux.sh
```


