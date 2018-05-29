---
layout: post
title:  "vim에서 스펠(spell) 체크하기 (오타 줄이기)"
categories: vim
---


다른 많은 IDE들이 스펠링 체크를 제공해 주는 것 처럼, vim도 spell check를 기본으로 제공한다.

## 기본 스펠체크 명령어
```
:set spell
```
이다. `set spell` 뒤에 `spelllang`로 해당 언어를 입력해주면 된다.

가장 많이 쓰이는 미국식 영어(?)로 스펠 체크를 하고자 한다면
```
:set spell spelllang=en_us
```
를 실행해주면 된다. 한 번도 스펠 체크를 한 적이 없다면 해당 언어의 파일을 다운로드 할 것이냐고 물어보는데, 확인을 누르면 자동으로 다운 받은 뒤 스펠 체크를 해준다. 

> 틀린 스펠을 하이라이팅 처리 해준다. 해당 언어 (여기서는 `en_us`)가 아닌 언어 (한글 등)도 모두 missspelling으로 감지한다는게 흠이지만, 스펠 검사 자체는 만족스럽다.

이를 끄고 싶다면
```
:set nospell
```

을 입력하면 된다.


### 특정 파일타입/파일명에서 스펠 체크
만약 특정 파일타입(파일명)에서만 스펠 체크를 해보고 싶다면 다음 설정을 `~/.vimrc`에 추가해 준다.

다음 예제는 `md`로 끝나는 모든 마크다운 파일에 스펠 체크를 하는 것이다.
```
autocmd BufRead, BufNewFile *.md setlocal spell
```

특정 파일 명에서만 스펠 체크를 할 수도 있다.

```
autocmd FileType gitcommit setlocal spell
```

### spell 자동 완성
스펠링이 틀린 해당 단어 위에서 입력모드(Insert mode)로 변환한 후에 `CTRL-N`이나 `CTRL-P`를 입력하면 해당 단어에 추천 단어 리스트가 나온다. 스펠링이 헷갈릴 때 사용하면 좋은 기능일듯.

complete에 `kspell`을 추가해주어야 한다.
```
set complete+=kspell
```
