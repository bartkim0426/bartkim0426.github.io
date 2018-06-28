---
layout: post
title:  "python: pyenv, virtualenv, auto in mac os"
categories: python
---



### Install pyenv

- using brew

```bash
brew install pyenv

# using your own profile
# use ~/.bashrc or ~/.bash_profile
# ~/.zshrc when using zsh
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
source ~/.bash_profile
```

### Install virtualenv

- using brew

```bash
brew install pyenv-virtualenv

echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```


### Install autoenv

- using brew

```
brew install autoenv

To finish the installation, source activate.sh in your shell:
  source /usr/local/opt/autoenv/activate.sh

# add path for about after source
echo 'source /usr/local/opt/autoenv/activate.sh' >> ~/.bash_profile
```
