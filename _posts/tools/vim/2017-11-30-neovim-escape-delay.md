---
layout: post
title:  "neovim에서 esc 지연 증상 해결 (mode 변경시 delay)"
categories: vim
tags: vim neovim
---

neovim을 사용하던 중, esc를 눌렀을 때 insert mode에서 normal mode로 전환하는 데 약간의 지연 증상이 발생하였다.

사용하기 어려운 정도는 아니지만, 계속 escㄹ르 누른 뒤에 hjkl 명령어 등이 입력이 되서 지워주어야 해서 관련 해결법을 찾아보았다.

### 해결책1. tmux
[neovim github issue](https://github.com/neovim/neovim/issues/2035) 중에 비슷한 이슈가 있는데, tmux를 사용할 때 그런 증상이 발생하면 다음 세팅을 `.tmux.conf`에 추가하라고 하여 추가시켜 보았다.

```bash
set -sg escape-time 0
```

> `tmux source-file ~/.tmux.conf`로 수정사항을 적용해주어야 한다.

