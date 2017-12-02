---
layout: post
title:  "python3 지원하는 vim 설치하기 (os x)"
categories: vim
tags: vim tools
---

신형 맥북을 사면서 vim 세팅들을 대부분 이전 맥북에서 옮겨왔지만, 몇몇 자동완성 플러그인(jedi 등)이 잘 작동하지 않아서 문제를 해결하던 중 python3가 포함되지 않은 vim이라는 것을 발견했다. 

자신의 vim configure를 확인하는 방법은, vim을 킨 뒤
```
:version
```
로 확인하는 방법과, 

bash에서 
```bash
$vim --version
# python만 학인하고 싶다면
$vim --version | grep "python"
```
으로 확인 가능하다. 나는 `brew install vim`으로 기본 세팅된 vim8이 깔려 있는데, python2는 +, python3는 -인 상태였다.


### brew로 시도
우선은 검색 결과 대부분이 os x에서는 brew로 설치해서 설치된 vim을 upgrade 해 보았다.
```bash
$brew upgrade vim --with-python3
```
결과는 already installed... 그래서 다음번에는 vim을 다 지운 뒤 `--with-python3` 설정을 넣어 설치해보았다.
```bash
$brew remove vim
$brew cleanup
$brew install vim --with-python3
```
대부분의 blog, SO에서 이 방법으로 python3 설정을 가진 vim을 설치했다고 하여 이 방법으로 설치가 될 줄 알았는데, 이번에는 `f_python3.c:75:10: fatal error: 'Python.h' file not found` 라는 에러가 발생하면서 vim이 설치되지 않았다. 

이상하게 `--with-python3`가 없으면 잘 설치가 되는데, python3 설정을 함께 설치하면 계속 동일한 에러가 발생하였다. 

brew를 update해도 그대로이고, 관련 글들을 찾아봐도 별로 없고, vim8 이후에 brew쪽에서 뭔가 이상한 것 같아서 brew가 아니라 vim을 내려받아 직접 설치해보기로 하였다.


### vim github에서 직접 설치
[vim 공식 github](https://github.com/vim/vim)에서 직접 소스를 내려받아 설치해보았다. os 별 설치 문서가 나와있어 그것을 보고 참고하여 설치하면 된다. 

우선 vim을 내려받는다. 
```bash
$git clone https://github.com/vim/vim.git
```

그 후 make 명령어를 사용하여 설치하기 전에, `~/.configure`를 사용해서 설치할 빔의 설정을 해 준다. 이때, python 2 혹은 python 3을 사용하고 싶으면 관련 설정을 해준 뒤 파이썬 경로를 지정해주면 된다. 
> 참고: [Youcompleteme 문서에 나온 vim 설정법](https://github.com/Valloric/YouCompleteMe/wiki/Building-Vim-from-source)

```bash
cd vim
./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes \
            --with-python-config-dir=/usr/lib/python2.7/config \
            --enable-python3interp=yes \
            --with-python3-config-dir=/usr/lib/python3.5/config \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 \
            --enable-cscope \
            --prefix=/usr/local
make VIMRUNTIMEDIR=/usr/local/share/vim/vim80
```

마지막으로 make 명령어를 사용해서 설치해준다.
```bash
# 이전에 make 명령어로 vim 패치지를 설치했다면 지워주자
make distclean
# 다음 두 명령여로 설치
# make && sudo make install 로 동시에 설치 가능
make
sudo make install
```

> 나는 이 과정에서 python3가 제대로 설정이 되지 않았다. 이유를 찾아보니 `python3-config-dir` 경로가 애매했기 때문이었다. os x에서는 대부분의 사람들이 python3를 brew로 설치하는데, 그럴 경오 python3의 경로는 `Cellar` 하위의 애매한 위치가 잡히고,  그래서 make 할 때 python3 경로를 제대로 못 잡아주는 게 아닌가 싶다. 
> 그래서 이번 기회에 `neovim`을 써 보기로 했고, neovim을 설치해서 사용 중이다. 다음에 기회가 된다면 neovim 사용기도 적도록 하겠다.
