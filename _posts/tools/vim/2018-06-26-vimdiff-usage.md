---
layout: post
title:  "vimdiff usage"
categories: vim
---


vimdiff is equivalent to `vim -d`.

It's easiest way to start editing diff mode with vim.


### Basic usage

```bash
vimdiff file1 file2
# equals to
vim -d file1 file2
```


### Cheat sheet

```vim
# remove leftover spacing issue
:diffupdate

# next / previous difference
]c :
[c :

# diff obtain
do

# diff put
dp

# open/close folded txt
zo
zc

# avoid whitespace comparision
:set diffopt+=iwhite
```



> [vimdiff documentation](http://vimdoc.sourceforge.net/htmldoc/diff.html)
