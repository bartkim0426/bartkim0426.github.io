---
layout: post
title:  "using peco to search termianl command"
categories: linux
---

### installing peco

```bash
brew tab peco/peco
brew install peco

# check peco version after installing
peco --version
```

### usage

use peco with other function by pipe
```bash
# search system log in OSX
cat /var/log/system.log | peco

# find process and kill
ps - ef | peco | awk '{ print $2 }' | xargs kill

# use ctrl + space for multiple choices

```
Can change search mode by `ctrl + r`

### use peco in termianl command history
(support zsh, oh-my-zsh)

- Save this script as `~/.zsh/peco-history.zsh`
```bash
# from http://qiita.com/uchiko/items/f6b1528d7362c9310da0 by uchiko

function peco-select-history() {
    local tac
    if which tac > /dev/null; then
        tac="tac"
    else
        tac="tail -r"
    fi
    BUFFER=$(\history -n 1 | \
        eval $tac | \
        peco --query "$LBUFFER")
    CURSOR=$#BUFFER
    zle clear-screen
}
zle -N peco-select-history
bindkey '^r' peco-select-history
```

- add `source ~/.zsh/peco-history.zsh` in your `~/.zshrc`
- then you can use `ctrl-r` to use peco search
- you can increase shell history by adding this to `~/.zshrc`
```bash
HISTSIZE=100000000
SAVEHIST=100000000
```
