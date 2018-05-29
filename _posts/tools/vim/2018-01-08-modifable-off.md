---
layout: post
title:  "E21: Cannot make changes, 'Modifiable' is off라고 나올 때"
categories: vim
---


`E21: Cannot make changes, 'Modifiable' is off`라는 에러가 나올 때 해결방법


```
:set modifiable
# 축약어로 다음도 사용 가능
:set ma

# 다시 modifiable을 끄고 싶다면
:set nomodifiable
# :set noma 도 가능
```
