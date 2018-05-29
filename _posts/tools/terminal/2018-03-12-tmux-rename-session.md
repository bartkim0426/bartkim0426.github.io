---
layout: post
title:  "tmux session 이름 변경하기"
categories: terminal
---


tmux session을 처음 생성할 때 만든 이름을 변경할 때는 두가지 방법이 있다.

### 1. shell에서 명령어로 변경

```bash
$tmux rename-session -t [current-session-name] [new-session-name]
```

### 2. tmux 내 커맨드에서 변경
tmux 내에서 커맨드 실행 : `ctrl + b + $` 혹은 `ctrl + b + :` => `rename-session -t [current-session-name] [new-session-name]`

> 만약 tmux hotkey를 `ctrl+b`에서 다른 키로 변경하였다면 변경한 키를 입력해주면 된다.
