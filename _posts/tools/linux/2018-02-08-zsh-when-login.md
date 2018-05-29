---
layout: post
title:  "Mac os X : termianl에서 자동으로 zsh 열기"
categories: linux
---

처음에 zsh를 세팅 해도, 터미널을 열면 자동으로 `bash`가 실행된다.

다음 명령어를 실행시키면 그 다음부터 터미널을 열었을 때 자동으로 `zsh`가 실행된다.

```bash
sudo sh -c “echo ‘/usr/local/bin/zsh’ >> /etc/shells”
chsh -s /usr/local/bin/zsh
```
