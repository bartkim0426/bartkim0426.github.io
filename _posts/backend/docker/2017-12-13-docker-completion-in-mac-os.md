---
layout: post 
title:  "Install docker completion in bash&zsh"
categories: docker
tags: docker
---


## bash에서 docker completion 사용하기
`brew`를 사용해서 설치해주면 된다.

```bash
$ brew install bash-completion
$ brew tap homebrew/completions
```

**`.bash_profile`**에 다음을 추가
```
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

**다음 명령어 실행**
```bash
ln -s /Applications/Docker.app/Contents/Resources/etc/docker.bash-completion /usr/local/etc/bash_completion.d/docker
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-machine.bash-completion /usr/local/etc/bash_completion.d/docker-machine
ln -s /Applications/Docker.app/Contents/Resources/etc/docker-compose.bash-completion /usr/local/etc/bash_completion.d/docker-compose
```

## zsh에서 docker completion 사용하기
`oh-my-zsh`를 사용하는 경우 `docker` 플러그인을 추가해주면 된다.

**`.zshrc`에 다음을 추가**
```
# 기존에 설치되 있는 것들은 그대로 두고 
# 가장 앞에 docker를 추가해존다.
# comma(,)로 분리하지 않고 whitespace(띄어쓰기)로 구분
plugins=(docker git)
```
**zsh 재실행**
```bash
$exec zsh
```
