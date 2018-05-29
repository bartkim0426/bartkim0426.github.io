---
layout: post
title:  "vim에서 tab을 space로 전환"
categories: vim
---

tab 캐릭터를 space로 전환하려면 vim 창에서 다음과 같이 해준다.

## `expandtab` 옵션: tab 캐릭터를 space로
```
:set expandtab
```

## `retab`: 현재 있는 탭을 space로 전환
```
:retab
```

## `shiftwidth`: `>`, `<`를 사용한 탭전환을 스페이스로
```
:set shiftwidth=4
# :set sw=4로 축약 가능
```

## `tabstop`: 띌 스페이스의 숫자
```
:set ts=4
# set ts=4로 축약 가능
```

한줄로 적으면 다음과 같이 된다.
```
:set ts=4 sw=4 expandtab
```


이를 vim 설정에 넣어주면 global로 적용이 된다.


다음 세팅을 `~/.vimrc`에 추가해준다. (`neovim`인 경우는 `~/.vim/init.vim`이나 본인이 직접 설정한 파일에 추가.)

```
set expandtab
set tabstop=4
set shiftwidth=4
```

이를 응용하면 파일별로 space를 2칸, 4칸 띄우게 설정하거나 직접 수정하는 것도 가능하다.
