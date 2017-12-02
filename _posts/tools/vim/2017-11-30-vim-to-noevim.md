---
layout: post
title:  "neovim 설치시 vimrc 세팅 가져오기"
categories: vim
tags: vim tools
---

지금까지는 순정 vim만 사용해보고, (8.0 버전을 메인으로 사용하였다.) macvim 등 프로그램으로 돌아가는 vim들은 한 번도 사용해 보지 않았다. 그러던 중 neovim에 대해 알게 되었고, 일반 vim과 크게 다르지 않고 훨씬 더 빠르다고 하여 neovim을 사용해 보게 되었다.

## Neovim 설치법
Mac의 경우에는 homebrew를 통해 설치가 가능하다.

```bash
$brew install neovim/neovim/neovim
```

python 패키지 설치 프로그램인 pip를 통해서도 설치가 가능하다. vim에서 python이 필요한 라이브러리를 사용한다던지, (jedi, python-mode 등) 파이썬 컴파일이 필요하면 pip로 설치해 주어야 한다. 
```bash
$pip install neovim
```


## Neovim 설정법
neovim은 vim과는 다르게 `vimrc` 파일이 아니라 `init.vim` 파일을 통해 설정을 관리한다. 경로는 조금씩 다를 수 있지만 대개 `~/.config`에 놓으면 적용된다.

> 만약 system에 `$XDG_CONFIG_HOME` path가 등록되어 있으믄 그것을 대신 사용하면 된다


나는 기존에 vim에서 사용하는 설정들이 있기 때문에 이를 neovimdㅡ로 옮기는 설정을 찾아보았다. 다행이도 nvim 자체 도움말에 관련된 설명이 있어서 참고하였다.

도움말은 다음과 같이 볼 수 있다.
```
# nvim 실행 후
:h nvim-from-vim
```

## Neovim으로 vim 설정 옮기기 (ln으로 바로가기 만들기)
```bash
mkdir -p ${XDG_CONFIG_HOME:=$HOME/.config}
ln -s ~/.vim $XDG_CONFIG_HOME/nvim
ln -s ~/.vimrc $XDG_CONFIG_HOME/nvim/init.vim
```

## Neovim 사용 후기
사실 지금까지는 제대로 neovim을 사용했다고 할 수 없어 정확한 평가를 내리기가 어렵다. 더욱이 neovim 설정을 하지 않고 vimrc 설정을 그대로 가져온 터라 여러 충돌이 나는 부분들도 있고, 기존 플러그인이 제대로 작동하지 않는 경우도 있다. 

무엇보다 normal mode로의 전환이 약간 느린 듯한 느낌인데, 하루빨리 neovim용 설정을 해봐야겠다.
