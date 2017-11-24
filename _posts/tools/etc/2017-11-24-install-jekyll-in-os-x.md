---
layout: post
title: "Install jekyll in os x"
categories: etc
tags: ruby jekyll
---

os x에는 기본으로 ruby 2.0이 깔려있다. 하지만 이걸로는 bundle을 사용할 수 없기 때문에 jekyll을 사용하기 위해서는 ruby를 업그레이드 시켜 줘야 한다. 여러가지 방법이 있지만 `rvm`을 사용해서 깔아주었다

> [`rvm`](https://rvm.io/): Ruby Version Manager. pyenv처럼 ruby verion을 관리할 수 있는 매니져

### Install rvm
```
# bash를 쓴다면 다음 명령어로 rvm 설치가 가능
\curl -sSL https://get.rvm.io | bash -s stable

# zsh를 쓴다면 다음 명령어를 
# ~/.zshrc 에 추가
[[ -s "$HOME/.rvm/scripts/rvm"   ]] && . "$HOME/.rvm/scripts/rvm" 
```


### install ruby 2.4
```
# 설치 가능한 루비 버전 확인
rvm list know
# 2.4.2 버전 설치
rvm install ruby-2.4.2 
```

### Set default ruby to 2.4
```
rvm use ruby-2.4.2 --default

# 루비 버전 확인
ruby -v
```

### install bundler
```
gem install bundler
```


### install jekyll by bundler
```
bundle install
```
