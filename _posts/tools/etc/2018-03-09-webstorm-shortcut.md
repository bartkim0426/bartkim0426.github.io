---
layout: post 
title:  "내가 보려고 만든 실용적인 webstorm 단축키"
categories: etc
---

원래는 프론트 코딩(html, css, js)들도 `vim`으로 하려고 무던히 노력중이었으나.. 


`javascript`에 대한 실력 부족(...)과 다양한 이유(vim plugin 속도 저하 등)로 프론트 작업을 할때는 다른 텍스트 에디터를 찾아보았다.


예전에 썼던 [`Atom`](https://atom.io/)과 [`sublime-text`](https://www.sublimetext.com/)나 프론트 개발자들이 자주 사용하는 [`bracket`](http://brackets.io/), [`visual studio`](https://www.visualstudio.com/ko/) 등도 후보였는데, 

결국은 `jetbrain`사에서 나온 [`webstorm`](https://www.jetbrains.com/webstorm/)을 사용해보기로 했다. 심지어 유료 에디터라 30일 free trial을 사용해보았다.


결론은 생각보다 만족스러웠다. `pycharm`은 생각보다 실망스럽고 너무 무거워서 조금 걱정을 했는데, `webstorm`은 생각보다 많이 무겁지 않았다. 

뿐만 아니라 기본적인 프론트 언어들의 자동완성 뿐 아니라 `React`, `Vue` 등 다양한 프론트 프레임워크의 자동완성 등도 지원하고 직관적이지는 않지만 다양한 정보를 가진 인터페이스도 괜찮았다. 
(아주 초심자의 경우는 `atom`이나 더 가벼운 텍스트 에디터가 더 나을 것 같다.)

무엇보다도 `vim` 단축키를 기본적으로 상당히 잘 구현해 놓은 것 같다. `sublime text`에도 `vintage` 모드가 있어서 vim 단축키를 지원하긴 하지만, 생각보다 잘 구현되지 않아서 조금 불편했었는데, `webstorm`은 정말 `vim`을 사용하는 것처럼 부드러운 사용성을 보였다. `vim`을 주로 사용하는 사람이라면 서브에디터로 사용할 법 하다. 



`:vs` 등 vim 단축키들을 모두 활용할 수 있는데, 이를 다양한 단축키와 함께 조합해 사용하면 생각보다 사용성이 좋다.

추후에 잊지 않고 보기 위해서 다양한 단축키들을 기록해놓는다.

```
# 탭 이동
Cmd + Shift + [, ]

# 새 파일 열기
Cmd + Shift + o

# 전체검색 (파일 뿐 아니라 모두 검색해줌. 가장 좋은 단축키인것같다.)
Shift + Shift

# 멀티라인 선택
alt + 마우스를릭

# 메뉴 열기/닫기
Cmd + b
```

추후에 또 유용한 단축키들을 찾으면 더 올리겠다.
