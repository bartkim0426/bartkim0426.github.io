---
layout: post 
title:  "highlight vue files in vim: vim-vue"
categories: vim
tags: vim vue plugin
---

vim에는 많은 syntax highlight가 있고, javascript, python 등 다양한 파일 (.js, .py)을 지원한다. 하지만 vue 파일을 만들면 `.vue` 확장자를 가지기 때문에 이를 인식하지 못한다. 이 확장자를 인식하게 만들 수도 있지만 vue가 가진 특수 syntax도 highlighting도 하고 싶어서 찾던 중 `vim-vue` 플러그인을 발견하여 설치해보았다.

> 자세한 설명 및 코드는 [vim-vue github](https://github.com/posva/vim-vue)에서 확인 가능하다.

### Install by vundle
다음 코드를 `~/.vimrc`에 넣어준다.

```
Plugin 'posva/vim-vue' 
```
그리고 source vimrc를 해준 후 해당 패키지를 install해준다.

(vim 창에서 명령어 모드로 다음 코맨드를 입력)
```
:so %
:PluginInstall
```

vim을 재실행시켜보면 `.vue` 확장자를 가진 파일에 syntax hightlighting이 되는 것을 볼 수 있다.
