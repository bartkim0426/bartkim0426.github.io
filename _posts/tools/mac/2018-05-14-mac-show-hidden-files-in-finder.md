---
layout: post
title:  "MAC : finder(파인더)에서 숨은 파일/폴더 보기"
categories: mac
---


### 일시적으로 확인하기

finder 창을 띄운 뒤에 다음 키를 입력하면 숨은 파일이나 폴더를 확인할 수 있다.

```
cmd + shift + .
```


### finder 설정 변경하기

`terminal`에서 다음 명령어를 입력해준다. (터미널은 `spotlight`에서 terminal을 검색해서 열 수 있다.)

```
defaults write com.apple.finder AppleShowAllFiles YES
```

이후 finder를 재시작해주면 된다. (docker의 finder 오른쪽클릭 => 재시작 (`relaunch`))


다시 hidden 파일을 숨기려면 YES 대신 NO 명령어를 입력해주면 된다.
```
defaults write com.apple.finder AppleShowAllFiles NO
```
