---
layout: post
title:  "Vim 에서 quote, unquote 하기"
categories: vim
---

가장 쉬운 방법은 `surround.vim` 플러그인을 사용하는 것이다. [깃허브](https://github.com/tpope/vim-surround)에 사용법이 나와있고 추후에 사용법을 정리 할 예정. 

하지만 플러그인을 사용하지 않고 vim스러운 방법도 있기에 공유한다.


## Quoting (''를 이용하여 word 감싸기)
```vim
ciw'Ctrl+r"'
```
각각의 명령어를 설명하자면
- `ciw`: 커서가 있는 단어를 대체 (change) 
- `'`: 작은 따움표 하나추가
- `Ctrl+r"`: `Ctrl+r`은 입력 모드에서 레지스터에 있는 것을 붙여넣어준다. `"` 레지스터는 최근에 복사/지운 레지스터기 때문에 `Ctrl+r"`은 방금 `ciw` 명령어로 지운 단어를 붙여넣어준다.
- `'`: 뒤에 따옴표 추가. (만약 따옴표를 자동으로 만들어 주는 플러그인을 사용한다면 따로 쓰지 않아도 된다.)

위의 명령어를 등록해 놓고 써도 된다. (사실 그러면 `surround.vim`을 사용하는 거랑 별 차이가 없기는 하다)

> 추가 명령어

`Ctrl+r"`는 repeat (`.`) 사용이 안되기 때문에 `<C-r><C-o>`를 사용하는 것이 좋다. 그럼 점 명령어로 계속 반복적으로 사용이 가능하다.
```vim
ciw'<C-r><C-o>"'
```

## Unquoting
```vim
di'hPl2x
```

## Change single (`'`) to douple (`"`)
```vim
va':s/\%V'\%V/"/g
```
