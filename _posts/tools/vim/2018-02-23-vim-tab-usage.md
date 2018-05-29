---
layout: post
title:  "Vim tab usage"
categories: vim
---

I use `tmux` and `vim`, so I just split my one vim tab and use multiple tmux window. 

But when using many window, it's little bit annoying to find specific window. So I'm trying to use vim tab function and summarize usage.


### commands
```
# new tab
# when no filename, just open blank tab
:tabnew [filename]

# close tab
:tabclose
:tabc

# next or previous tab
:tabnext
:tabn
:tabprevious
:tabp
# or 'gt', 'gT'in normal mode
# '{i}gt' goes to position i

# split
:tab ball # show each buffer in a tab
:tab split  # copy current window to new tab
```
