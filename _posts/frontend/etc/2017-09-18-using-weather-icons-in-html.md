---
layout: post
title:  "Using weather icons in html"
categories: frontend
tags: frontend weather icon
---

html에서 날씨를 표시할 일이 생겨 찾아보던 중, 날씨 아이콘을 쉽고 간편하게 사용할 수 있는 프로젝트가 있어 짧게 작성한다.

## Using [weather-icons](https://github.com/erikflowers/weather-icons)

1. [weather-icons 공식 홈페이지](https://erikflowers.github.io/weather-icons/)에 들어가 `Download Now`로 아이콘 zip 파일을 다운받은 후 압축을 풀어준다.

2. css 디렉토리 아래의 `weather-icons.min.css`을 프로젝트의 css 폴더에 추가해준다.

3. font 디렉토리 아래의 3개의 폰트 파일(`.ttf, .woff, .woff2`)을 프로젝트의 font 폴더에 추가해준다.

4. `<i class="wi wi-day-sunny"></i>`와 같이 쉽게 사용 가능하다.
