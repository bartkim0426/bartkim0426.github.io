---
layout: post
title:  "linux command lecture"
categories: lecture
---


### Man page
- Manual pages
- `man cowsay`

### Filesystem
- filename; except slash `/`
- Use quoted or escaped for contains space
- Filesystem tree; `(root)`
- `pwd`; current working directory

#### Globbing
- use `*` like `ls *log`
- use `?` for one char
- `{}`; or
- `[]`



## Linux

- `Free as a beer`; money
- `Free as in speech`; improve, modify, redistribute ...

### Distribution
- Redhat; 기업들에게 인기, not free as a beer but receive service
- ubuntu; free, updated & changed frequently / use for many device (desktops, laptops)
- debian; stable but slow updated
- linux mint; desktop users 
- coreOS; apps

### Important directories
- `etc`; configuration files
- `var`; variable files
- `bin`; executable binaries file
- `sbin`; admin binaries
- `lib`
- `usr`; user program

### `$PATH`
- `echo $PATH`
- linux find dirs for find executable files

### Linux security
- superuser; user name `root`, using by `sudo` command
- `sudo` vs `su`;

### Package
- package source list; software for linux
- `/etc/apt/sources.list`
- upgrade packages; `sudo apt-get update`; not change, just show availabe lastest software
- `sudo apt-get upgrade`; actually update software
- discover packages in [here](https://packages.ubuntu.com/)

#### finger
- `finger`; user info lookup program
- `cat /etc/passwd`; 
- `root:x:0:0:root:/root:/bin/bash`; in linux system, linux user always has 0 for uid, gid

### User management
- Create user; `adduser student`
- Check with `finger student`
- Login with other user in docker; `docker exec -it --user student 7c95 bash`
- `student is not in the sudoers file.  This incident will be reported.`; 
- `cat /etc/sudoers`; can see user groups
- `#includedir /etc/sudoers.d`; 이 폴더에 있는 user에 sudo permission 부여
- Give sudo access; add file in `/etc/sudoers.d/` (File name isn't important)

```
student ALL=(ALL) NOPASSWD:ALL
```

### Other auth method
- key based authentication; Public key encryption
- Public key; is a box / server send message
- Private key; is a key / send message by private key and server received with public key

### Generating key pairs
- `ssh-keygen`; support `DSA`, `ECDSA`, `ED25519`, `RSA`
- local 머신에서 `ssh-keygen`; ex)) ~/.ssh/linuxCourse
- `cat ~/.ssh/linuxCourse.pub`의 내용을 서버 pc의 `~/.ssh/authorized_keys`에 추가
- permission을 주는게 좋음: `chmod 700 .ssh`, `chmod 644 .ssh/authorized_keys`
- 이후 서버 pc에 private key로 접속 가능하다. `ssh student@your_ip_number -i ~/.ssh/linuxCourse`

### Forcing key based authentication
- `/etc/ssh/sshd_config`
- `passwordAuthentication no`로 변경해주면 됨
- `sudo service ssh restart` 이후에는 password로 로그인 불가

### File permission
```
-rw-r--r-- 1 student student  807 Oct  9 17:52 .profile
drwx------ 2 student student 4096 Oct 10 13:14 .ssh
```
- owner / group / everyone
- group; automatically create group when making user

### Octal permission
- r = 4 / w = 2 / x = 1
- ie) rx => 5

### chgrp and chown
- `chown`; change owner
- `chgrp`; change group

### Intro to ports
- http (80), https(443), ssh(22), ftp(21) ...
- Firewall; can config which port is open

### UFW
- `sudo ufw status`; 
- `sudo ufw default deny incoming` ; block all incoming request, include ssh and so on...
- `sudo ufw default allow outgoing`
- `sudo ufw allow ssh`, `sudo ufw allow www` (open ssh connection)
- `sudo ufw enable` => status active (can see by `sudo ufw status`)

## Vagrant

```
vagrant init ubuntu/trusty64

vagrant status
vagrant suspend
vagrant up
vagrant ssh
vagrant halt
vagrant destroy
```

