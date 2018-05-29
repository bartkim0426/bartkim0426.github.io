---
layout: post
title:  "Install and using vim plug"
categories: vim
---

I used `Vundle` when using vim, but I found bit more great plugin installer - `Plug`.

Easy to install. 


### When using vim, 
```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### When using neovim
```bash
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```


## Usage
here's my example plugs
```vim
call plug#begin('~/.vim/plugged')
if has('nvim')
  Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }
else
  Plug 'Shougo/deoplete.nvim'
  Plug 'roxma/nvim-yarp'
  Plug 'roxma/vim-hug-neovim-rpc'
endif
call plug#end()
```

Go into command mode using `:` and put commands

```vim
# install plugins
:PlugInstall [name...]
# update plugins
:PlugUpdate [name...]
# remove unused plugins
:PlugClean

# other commands
:PlugUpgrade # upgrade plug itself
:PlugStatus
:PlugDiff
:PlugSnapshot
```

