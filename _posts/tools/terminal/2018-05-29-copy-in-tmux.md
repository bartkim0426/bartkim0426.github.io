---
layout: post
title:  "tmux : use capture in tmux"
categories: tmux
---


```bash
# capture 30000 line
tmux capture-pane -pS -30000

tmux capture-pane -S -30000 \; save-buffer - \; delete-buffer

tmux capture-pane -J -p -t %123 > file.txt
```


### Go to copy mode

use tmux prefix (`ctrl + b` with default) with `[`


Then you can access to copy (vim) mode...

```bash
tmux-prifix + [
```
