---
layout: post 
title:  "Debugging in vue.js"
categories: vue
tags: vue
---

vue에서 디버깅하는 두 가지 방법

1. chrome의 개발자 도구 사용
vue-cli(webpack)을 사용하는 경우 크롬의 개발자 도구를 활용하여 쉽게 디버깅 가능.

2. vue dev tools (https://github.com/vuejs/vue-devtools)
vue dev tools을 설치하면 크롬 개발자 도구에서 뷰 컴포넌트, 이벤트 등을 쉽게 확인이 가능하다. 

설치도 `extension` 형태로 아주 쉽게 설치할 수 있다. (현재 `Chrome`, `Safari`, `Firefox`를 지원)

- [크롬 extension 설치](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
- [파이어폭스 애드온 설치](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
- [사파리 workaround 설치](https://github.com/vuejs/vue-devtools/blob/master/docs/workaround-for-safari.md): `npm install`이 필요하다.

이후 개발자 모드로 들어가 보면 `Vue`탭이 생긴 것을 볼 수 있다. 

![vue-dev-tools](https://i.imgur.com/hRlfggf.png){:class="img-responsive"}
