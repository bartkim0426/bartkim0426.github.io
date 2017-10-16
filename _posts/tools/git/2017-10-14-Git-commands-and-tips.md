---
layout: post
title:  "Git commands and tips"
categories: git
tag: tips git
---


## How to turn on git autocorrect
```bash
git config --global help.autocorrect 5
```

## Activate rerere (easy to use merge)
```bash
git config --global rerere.enabled true
```

## `git apply --check`
patch 적용 전 패치가 적용되는지 시험 가능

## Git rebase
```bash
git rebase --onto master experiment sckim
```

## Git 공백으로 인한 커밋 방지하기
```bash
git diff --check
```

## Git 대화형 명령어
명령어 뒤에 `-i`를 붙이면 대화형으로 shell에 창이 나온다. (하지만 쓰다보니 그냥 사용하는것이 더 편리한듯하다.)


예시)
```bash
git add -i
```

## Git stash
깃 사용 중에 수정중인 사항을 모두 잠시 제껴두고 pull을 하고 싶다면 stash 명령어를 사용하면 된다.

```bash
git stash
# branch로 만들고 싶으면
git stash branch branchname

# stash한 사항 적용시키기
git stash apply
# add 된 status 까지 적용시키려면
git stash apply --index
```

## 특정 폴더에서 특정한 파일만 .gitignore에서 제외시키기

```bash
# exclude everything
somefolder/*

# exception to the rule
!somefolder/.gitkeep 
```

## Github을 리셋한 후 현재 direcotry 기준으로 push하기

```bash
git fetch origin
git reset --hard origin/master
git clean -f -d
```

## Git tag 달기
쉽게 해당 시점으로 돌아갈 수 있게 태그를 달 수 있다.

```bash
git tag -s v1.5 -m 'my signed 1.5 tag

# 공개키 파일 선택
gpg --list-keys
```
