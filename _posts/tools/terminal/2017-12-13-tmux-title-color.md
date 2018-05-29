---
layout: post
title:  "item2 title(메뉴바) 색깔 변경하기"
categories: terminal
tag: terminal item2
---

iterm2 color scheme를 변경하더라도 기본 tmux title(위의 메뉴 바) 색깔은 그대로라 항상 보기 거슬렀는데 우연히 어느 강의에서 iterm2의 메뉴 색깔이 검정으로 되어있는 걸 보고 나도 바꿔야겠다 싶어서 찾아보았다. 

[iterm2 공식 document](https://iterm2.com/documentation-escape-codes.html)에 떡하니 나와있다.
0~255의 숫자를 넣어서 rgb로 지정해주면 된다. 다음은 document에 나와있는 purple 색상 예시.

```bash
echo -e "\033]6;1;bg;red;brightness;255\a"
echo -e "\033]6;1;bg;green;brightness;0\a"
echo -e "\033]6;1;bg;blue;brightness;255\a"
```

다음 명령어로 한줄로 넣어도 괜찮다.
```
printf -- $'\033]6;1;bg;red;brightness;20\a\033]6;1;bg;green;brightness;20\a\033]6;1;bg;blue;brightness;20\a'
```
