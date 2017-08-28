---
layout: post
title:  "손에 잡히는 vim ( getting-started-with-vim )"
categories: vim
tags: vim tools books
---

인터넷에 추천 글을 보고 도서관에서 빌려본 '손에 잡히는 vim'

총 150여페이지밖에 안 될 정도로 짧아서 며칠만에 다 볼 수 있었다.   
프로그래밍을 처음 시작하고 처음 vim을 사용할 때의 그 두려움은 정말 어마어마했다. 
vim 자체가 워낙 생소하기도 했지만, 아마 vim에 대해서 명확하게 알지 못하고 그저 인터넷을 보고, 키를 눌러보며 하나하나 살얼음을 걸었기 때문이였던 것 같다.  

그런 관점에서 이 책은 vim을 처음 접하는 초보자들에게 상당히 (어쩌면 유일한) 괜찮은 책이라고 할 수 있다. 특히 초반에 vim의 기본에 대한 설명들은 어느 정도 vim에 대한 기초를 잡아줄 법 하다. (물론 인터넷에도 비슷한 정보는 널려있다.)

다만 150여 페이지밖에 안되는 적은 공간 때문인지 많은 정보를 담지 못했고, `:help`를 사용해서 찾을 수 있는 방법을 제시했음에도 불구하고 초심자들이 이 책만으로 vim에 어느정도 가까워졌다는 느낌을 갖긴 힘들듯하다.  

vim을 처음 접하는 사람, 어느 정도 vim으로 기본 작업은 해봤지만 vim의 기본을 알고 싶은 사람들에게 추천한다. 

또한 매크로 등 초심자들에게는 생소한(?) 내용들을 나름 쉽게 다뤘기 때문에 vim에 흥미를 돋궈주는, vim 공부에 초석을 다져주는 책이 될 수 있지 않을 까 싶다.

그러나 정말 깊은 수준의 vim을 익히고 싶으면 `practical vim`을 추천한다.

아래는 손에 잡히는 빔을 읽고 내가 잘 모르거나 기억해둘 만한 것들을 기록해놓았다. 책에 내용을 충실하게 반영한 것도 아니고,  저작권 문제로 예제 등은 적지 않거나 나만의 예제로 대체하였다.


### 도움말 보기
```
# 도움말에 사용되는 접두어들

# normal mode- 따로 없음
:help x
# insert mode - i_
:help i_<Ctrl-N>
# command mode - :
:help :w
# visual mode - v_
:help v_u
# vim 실행 인수(?)- -
:help -r
# options
:help 'tabstop'
# command mode의 특수키
:help c_CTRL-B
```

### 도움말 내에서 tag 이동 명령어
```
# 커서 위치한 title(tag)로 이동
Ctrl-]
# back to the prev title(tag)
Ctrl-T
# show lists of all moved titles(tags)
:tags
```

### center, right align
```
:center, :right
```

### Substitute usages
구분자는 ,외에 `/`등도 가능하다
```bash
# substitute example
# :[range],hello,world,{option}
:%s,hello,world,g

# options for Substitute
g # global
i # ignore case
c # confirm
e # ignore error
```

### up/down numbers
```
<Ctrl + A> # (증가) 
<Ctrl + X> # (감소) 
```

### turn off highlight when searching
```bash
:nohl
:noh
```

### .vimrc의 약어(abbreviation) 설정
ab로 약어를 설정해 놓으면 입력 후 `<space>,` `<tab>`을 입력해서 바꿀 수 있음.  
`<Ctrl-V>`를 입력하면 약어 변환 안함  
`<Ctrl-]>`

```bash
ab bart bartkim0426@gmail.com

# ab commads
:ab [ihs]
:ab {ihs} {rhs}
:unab {ihs}
:abclear # clear all ab
:ia {ihs} {rhs} # only insert mode
:ca {ihs} {rhs} # command mode only
```

### enter registers
```bash
# register with func
"" # recent yank, delete
"0 # recent yank
"- # recent delete in line
"/ 
":
".
"%
"#

# basic register
"{reg}y{motion}
"{reg}p
"{reg}P # 커서의 앞부분에 추가
"{reg}d{motion}
Ctrl-R{reg} # only insert mode
```

### etc
```bash
# update + quit command
ZZ, :x  # :up + :q 

# open file with taps
vim -p file1 file2

# vim diff
vim -d file1 file2
```


### 단축키 설정 명령어ㅜ
```bash
:map
:nmap # normal mode
:imap # insert mode
:cmap
:vmap

:unmap # nunmap, iunmap ...
```

### autocmd
`autocmd(au)`란? 자동 명령, 
`au[tocmd] {event} {command}`로 사용
ex) `au FileType cpp colo slate`

더 자세한 내용은 autocommand, autocmd-events 항목에서 확인

### 반복작업 recording
```bash
# starting recording to register 'a'
# only using 소문자, 대문자 사용시 이미 존재하는 registry에 추가
qa

# ending recording
q

# play recording from register 'a'
@a
@@

# check recording
:reg a
``` 
here's an example - with using `a.txt`, `b.txt`

```
# a.txt
1234, cheese bugger, coke, ice-cream
5678, pasta, milk, gelato
1010, pizza, lemonade, choco-cake

# b.txt
1234, $350
5678, $550
1010, $500

# 명령어
qa
^
yiw
<ctrl-W><ctrl-W> # moving file
/<ctrl-R>" # copy recent registry
2w # jump two words
y$ # copy to the end
<Ctrl-W><Ctrl-W>
A,
p
q
```

### plugins => 추후 다른 문서에 정리

