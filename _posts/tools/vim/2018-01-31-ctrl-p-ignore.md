---
layout: post
title:  "vim Ctrlp 설치 및 설정(.pyc, node_modules 무시하기)"
categories: vim
---

`Ctrlp`는 vim 내에서 파일을 쉽게 검색해서 해당 파일을 열게 해주는 플러그인이다. 

해당 파일명만 입력하면 쉽게 파일로 갈 수 있기 때문에 `vim`을 사용할 때 자주 사용하는 플러그인 중 하나이다. 

[Ctrlp 공식 깃허브](https://github.com/kien/ctrlp.vim)에서 사용법과 설명이 소개 되어 있다.


우선 설치 방법이다. `Plug`를 사용하여 설치하면 된다.

```bash
call plug#begin('~/.vim/plugged')
Plug 'ctrlpvim/ctrlp.vim'               "Ctrl + P for search file
call plug#end()
```

이후 `source ~/.vim/init.vim` (일반 vim의 경우에는 `source ~/.vimrc`)를 해 주고

`PlugInstall`로 `ctrlp`를 설치 해준다.

> 자세한 `Plug` 사용법은 따로 포스팅 할 예정이다.


`Ctrlp`가 설치가 되었다면, 별다른 설정 없이 `Ctrl + p` 조합으로 해당 기능을 사용 할 수 있다.


![ctrlp](https://i.imgur.com/Hg1QOb9.png){:class="img-responsive"}


하지만 root나 home 디렉토리같이 하위 파일이 많거나, `*pyc`, `node_modules` 등 검색하고 싶지 않은 디렉토리/파일들도 검색이 되게 되면 원하는 파일 검색을 잘 못하거나 검색이 늦어질 수 있다.

이를 방지하기 위해 공식 github에서 각 OS별로 추가할 `wildignore` 설정을 제공해준다.

`~/.vimrc`나 `~/.vim/init.vim` (vim 설정 파일)에 다음을 추가해준다.
```bash
set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " MacOSX/Linux
set wildignore+=*\\tmp\\*,*.swp,*.zip,*.exe  " Windows
```

이 외에도 자신이 무시하고 싶은 파일/폴더를 추가해 줄 수 있다.

우선 `.gitignore`에 있는 파일을 무시하는 설정이다.
```bash
let g:ctrlp_user_command = ['.git/', 'git --git-dir=%s/.git ls-files -oc --exclude-standard']       "Ignore in .gitignore
```

`node_modules`, `.git` 폴더를 무시하는 설정이다.
```bash
let g:ctrlp_custom_ignore = 'node_modules\|DS_Store\|git'                                           "Ignore node_modules
```

`.pyc` 파일 등을 무시하고 싶다면 다음 설정을 추가해준다.
```bash
let g:ctrlp_custom_ignore = {
  \ 'file': '\v\.(pyc|so|dll)$',
  \ }
```

이제 터미널을 다시 시작하거나 `source ~/.vimrc`를 해주면 해당 파일들을 빼고 훨씬 빠르고 쾌적한 검색이 되는 것을 볼 수 있다.
