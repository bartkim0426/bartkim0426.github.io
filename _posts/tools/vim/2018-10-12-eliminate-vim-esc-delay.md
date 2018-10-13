---
layout: post
title:  "Eliminating vim ESC key delay"
categories: vim
---


After upgrade my mac to high sierra, I figured out that there's little delay for after ESC key in vim.

I usually use vim for development and blogging, so it's quite a big problem.


After googling, I found solution for this.

### In vim config
- `~/.vimrc` or your vim config, add this line

```
set timeoutlen=1000 ttimeoutlen=0
```

- timeoutlen is used for mapping delays, and ttimeoutlen is used for key code delays.
- You can check docs [here](http://www.polarhome.com/vim/manual/v57/options.html#'timeoutlen')


After add `timeoutlen` settings, the problem perfectly gone!


In addition to this solution, there's more for `zsh` and `tmux` users.

In your zsh setting, add (`~/.zshrc`)

```
# 10ms for key sequences in zsh
KEYTIMEOUT=1
```

And also in your tmux setting, (`~/.tmux.conf`)

```
set -s escape-time 0
```

> It's from [stackexchange answer](https://unix.stackexchange.com/questions/23138/esc-key-causes-a-small-delay-in-terminal-due-to-its-alt-behavior/25638#25638)
> Also don't forget `tmux source-file ~/.tmux.conf` for appling.
