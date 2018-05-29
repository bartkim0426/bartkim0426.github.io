---
layout: post 
title:  "CSS 색상 단위 -Name, RGB, RGBA, HSL, HSLA"
categories: vue
---

Sass에서는 다음의 우선순위를 따라서 색을 입력하라고 얘기한다. 인간의 두뇌로 이해하기 쉽고 작성자가 색을 변경하기 쉬운 순서대로 사용하라고 하는 것 같다.

1. CSS 색키워드: [여기](https://www.w3.org/TR/css-color-3/#svg-color)서 모든 목록을 확인 가능하다. (ex `black`, `red` ...)
2. [HSL 표기법](https://en.wikipedia.org/wiki/HSL_and_HSV) : hue, saturation, lightness를 사용. (`{backgrount-color: hsl(120, 100%, 50%)}`)
3. [RGB 표기법](https://en.wikipedia.org/wiki/RGB_color_model): 우리가 알고있는 그 RGB. (ex. rgb`(255, 0, 0)`)
> RGB 표기는 0~255 숫자 혹은 0%~100%의 백분율을 사용한다. `rgb(100%, 0%, 0%)`는 `rgb(255, 0, 0)`과동일
4. 16진법 표기법: `#000`

추가적으로 `Alaph`(투명도)가 추가된 `RGBA`, `HSLA`가 있다.

5. `HSLA`: hue, saturation, lightness + alaph (ex `hsla(120, 100%, 25%, 0.3)`)
6. `RGBA`: (ex `rgba(255, 0, 0, 0.3)`)
