---
layout: post
title:  "VIM: using ctags in vim"
categories: vim
---

### Installing ctags

In OS X, use `homebrew`

```
brew install ctags
```


### Make ctag index

In the directory you want to index, do command

```
ctags -R .
```

If want to hide tag index file, use `.` or inside `.git`

```
ctags -R ./.git/tags .
```


### Vim ctag command

![vim catg command](https://i.imgur.com/3t9CFGz.png){:class='img-responsive'}
