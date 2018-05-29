---
layout: post
title:  "mac os X, tmux 2.6(신버전)에서 yank, paste"
categories: mac
---


tmux를 새로운 버전으로 업데이트 하고 나서부터 vim yank, paste 키가 먹히지 않아서 찾아보던 중 업데이트 이후 key bind 명령어가 바뀐 것을 발견하고 수정하였다.

혹시 비슷한 문제를 겪는 사람이 있을 것 같아 공유한다.

### 이전 방식
```bash
bind-key -t vi-edit Up history-up
bind-key -t vi-edit Down history-down
unbind-key -t vi-copy Space     ;   bind-key -t vi-copy v begin-selection
unbind-key -t vi-copy Enter     ;   bind-key -t vi-copy y copy-pipe "reattach-to-user-namespace pbcopy"
unbind-key -t vi-copy C-v       ;   bind-key -t vi-copy C-v rectangle-toggle
unbind-key -t vi-copy [         ;   bind-key -t vi-copy [ begin-selection
unbind-key -t vi-copy ]         ;   bind-key -t vi-copy ] copy-selection
```

### 현재 방식
```bash
bind-key -T edit-mode-vi Up send-keys -X history-up
bind-key -T edit-mode-vi Down send-keys -X history-down
unbind-key -T copy-mode-vi Space     ;   bind-key -T copy-mode-vi v send-keys -X begin-selection
unbind-key -T copy-mode-vi Enter     ;   bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "reattach-to-user-namespace pbcopy"
unbind-key -T copy-mode-vi C-v       ;   bind-key -T copy-mode-vi C-v send-keys -X rectangle-toggle
unbind-key -T copy-mode-vi [         ;   bind-key -T copy-mode-vi [ send-keys -X begin-selection
unbind-key -T copy-mode-vi ]         ;   bind-key -T copy-mode-vi ] send-keys -X copy-selection
```
