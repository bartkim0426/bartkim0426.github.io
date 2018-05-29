---
layout: post
title:  "맥(OSX )에서 ctags 설치 및 사용"
categories: mac
tags: mac httpie
---


기본적으로 mac에는ctags가 깔려 있다. `ctags --help` 명령어를 쳐보면 확인이 가능하다. 그런데 `ctags -R`과 같은 기본적인 명령어를 실행시키면 `illegal option -- R`라는 오류가 나오는 것을 볼 수 있다.

그래서 `brew`를 통해 `ctags`를 재설치해주어야 할 필요가 있다.

```bash
brew install ctags
```

그리고 기존의 `ctags` 명령어를 `brew`를 통해 설치된 명령어로 대체한다. (`~/.bashrc`, `~/.zshrc` 등의 파일에 추가)

```bash
alias ctags="`brew --prefix`/bin/ctags"
```
그럼 이후부터 기본 `ctags` 명령어가 `brew`로 설치한 `ctags`를 실행시킨다.


## ctags 생성
직접 원하는 파일들만 tags 파일로 만들 수 있지만, 번거롭기 때문에 `-R` 명령어를 넣어서 만들어준다.
```bash
ctags -R
```

만약 제외시킬 파일들 (`node_modules`, `.git`, `*.js`)이 있다면 `--exclude` 명령어로 제외시킬 수 있다.
```bash
ctags -R --exclude=.git --exclude='*.html' --exclude='*.js' --exclude='*.css' --exclude=test --python=kinds=-i
```

> `--python=kinds=-i`는 `import`를 제외시키는 명령어. `ctags --list-kinds=python`은 추가/제거 가능한 파이썬 kinds를 확인 가능하다.

다음은 그 목록들.

```
c  classes
f  functions
m  class members
v  variables
i  imports
```

그럼 해당 디렉토리에 `tags` 파일이 생성된 것을 확인 가능하다.

## Tag file 사용법
전에 만들어진 `tags` 파일을 연 상태에서 다음 명령어로 해당 태그로 갈 수 있다.
```vim
:tj <tag_name>
# 분할된 창으로 열고 싶다면
# :stj <tag_name>
```

다시 tags 파일로 돌아가고 싶다면 
```vim
:po
```


## 단축키
- `:tj` 대신 `Ctrl + ]`
- `:po` 대신 `Ctrl + t`


## Vim 에디터와 연동하기
하지만 매번 이렇게 `tags` 파일을 열어서 작업하는 것은 매우 비효율적이기 때문에, vim 설정 (`~/.vimrc`, neovim의 경우에는 `~/.vim/init.vim`)에 tags 파일을 지정해 줄 수 있다.

다음 설정을 추가하면 해당 디렉토리에 `tags` 파일을 등록해준다.
```vim
set tags=./tags,tags
```

이제 `tags`가 있는 디렉토리에서 vim으로 작업을 한다면 `ctags`를 사용할 수 있다.

## ctags 명령어 수정
나는 `<C-]` 명령어는 편해서 그대로 사용하고 있고, `<C-t>`(돌아가기) 명령어만 `<C-[>`로 다시 매핑해서 사용중이다.

원하는 방식으로 변경해서 사용하면 될 듯 하다.
```vim
" ================================ Ctags settings ===================================
set tags=./tags,tags                "find tags in current dir
nnoremap <C-[> <Esc>:po<CR>         "Set Ctrl + [ to previous
```
