---
layout: post
title:  "VIM: git 커밋시 72자 넘어가는 부분 하이라이팅하기"
categories: vim
---


우선 vim에서의 git commit 메세지는 `syntax` 디렉토리의  `gitcommit.vim` 파일에서 설정되어있다.


> 해당 directory는 보통 `~/.vim` 안에 있지만 neovim 등은 아닐 수도 있다. 그럴 경우 vim 창에서 `:echo $VIMRUNTIME` 명령어를 실행시킨 후 나온 디렉토리에 들어가면 찾을 수 있다. 


`.../syntax/gitcommit.vim` 파일에 다음을 추가해준다.

```
...
syn clear gitcommitSummary
syn match gitcommitSummary    "^.\{0,72\}" contained containedin=gitcommitFirstLine nextgroup=gitcommitOverflow contains=@Spell
```

첫 번째 줄의 72자가 넘어가면 색상을 변경시켜 줄 뿐 아니라 자동으로 줄바꿈 처리하여 commit 메세지를 잘 지킬 수 있게 해준다.

![example image](https://i.imgur.com/cYgDGx6.png){:class="img-responsive"}


