---
layout: post
title:  "Install papercolor in vim"
categories: vim
---

## Install

when using `Vundle`
```vim
Plugin 'NLKNguyen/papercolor-theme'
```

Then add it to `~/.vimrc`
```vim
set t_Co=256   " This is may or may not needed.

set background=light
# if you want to use dark version
# set background=dark
colorscheme PaperColor
```

Done!! Easy usage!
