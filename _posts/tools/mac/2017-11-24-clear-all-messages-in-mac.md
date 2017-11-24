---
layout: post
title: "Clear all messages in mac"
categories: mac
tags: mac messages
---

1. Mac의 메세지 앱 종료

2. `~/Library/Messages` finder 열기

> finder에서 찾아도 되고, `Command+Shift+G`로 `~/Library/Messages`를 쳐서 들어가도 된다.
3. `chat.db`로 시작하는 3개의 파일을 지운다. (chat.db, chat.db-shm, chat.db-wal)
4. 다시 message를 실행
