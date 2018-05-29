---
layout: post
title:  "리눅스 파일 내용 검색하기"
categories: linux
---

터미널에서 해당 파일 안의 내용을 검색하는 방법을 찾아서 까먹지 않기 위해 공유한다.

`<pattern>` 부분에 원하는 검색 내용을 넣으면 된다.

```bash
grep -rnw `./` -e <pattern>
```
