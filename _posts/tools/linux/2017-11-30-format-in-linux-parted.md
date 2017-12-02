---
layout: post
title:  "parted: ubuntu에서 데이터 지우기"
categories: linux
tags: linux parted command
---

ubuntu 14.04 서버의 데이터를 모두 날려야 할 상황이 생겨서, 데이터를 지우는 linux 명령어들을 살펴보았다.

데이터를 날리는 방법은 크게 두가지로 나눌 수 있따.

## 1. 디스크 자체를 포맷하는 법과,
만약 어떤 이유로든지 현재 os를 모두 날리고 새로운 os를 설치하고 싶다면 fdisk, parted 등의 명령어를 사용하면 된다
> [SO](https://askubuntu.com/questions/517354/terminal-method-of-formatting-storage-drive) 참고

## 2. user와 해당 user의 모든 데이터를 삭제하는 방법
os는 그대로 둔 채 user와 그 데이터들만 지우고 싶다면 `deluser` 명령어를 사용하면 된다

```bash
sudo userdel -f -r username
```
