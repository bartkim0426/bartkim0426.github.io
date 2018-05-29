---
layout: post
title:  "Git tracking file/direcotry 제거하기"
categories: git
---


git을 쓰면서 git에 올리고 싶지 않은 파일/폴더들은 `.gitignore`에 넣어서 git tracking을 방지할 수 있다.

하지만 실수로 add를 해버렸거나 이미 git으로 관리중인 파일/폴더를 제거하고 싶을 때가 있는데, 그럴땐 `rm` 명령어를 사용해서 git에서 제거해주면 된다.

(물론 `gitignore`에도 추가해 주어야 다음부터 다시 추가되지 않는다.)
```bash
git rm -r --cached whatever_you_want/
```

