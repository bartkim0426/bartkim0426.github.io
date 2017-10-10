---
layout: post
title:  "Basic ubuntu 16.04 setting - pyenv, virtualenv, autoenv, postgres"
categories: python
tags: python pyenv virtualenv autoenv ubuntu
---

## Install pyenv
```
# install requirements
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev

git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# If using zsh, use `zshrc` instead of bash_profile
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
source ~/.bash_profile

# check pyenv installed well
pyenv versions
# test installing 
pyenv install 3.6.0
```
만약 설치가 되지 않고 오류가 발생한다면, 깃헙의 [common problem](https://github.com/pyenv/pyenv/wiki/Common-build-problems)을 참고하면 많은 도움이 된다.


## Install pyenv-virtualenv
```
git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
source ~/.bash_profile
```

## Testing pyenv-virtualenv
```
pyenv virtualenv 3.6.0 test-env
pyenv versions
pyenv activate test-env
pyenv deactivate
```

## Install autoenv
```
git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv 
echo 'source ~/.autoenv/activate.sh' >> ~/.bash_profile
source ~/.bash_profile
```


## Install postgres
```
sudo apt-get install postgresql postgresql-contrib

# access postgres user
sudo -u postgres psql
psql

```
