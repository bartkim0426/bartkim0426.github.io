---
layout: post 
title:  "zip, unzip 명령어 사용하기"
categories: linux
tags: linux zip
---

ssh 서버에서 용량이 큰 폴더를 그대로 옮겨야 할 일이생겨서 폴더를 그대로 옮기려고 했더니 시간이 엄청 오래걸렸다. 그래서 zip으로 압축을 해서 옮기기 위해 zip 명령어를 사용해 보았다. 사용법은 매우 간단.



**압축하기**
```bash
# 특정 파일을 filename.zip으로 압축
zip filename.zip file1 file2 file3

# dir1 directory를 압축
zip -r filename.zip dir1
```


**압축풀기**
```bash
# filename 이라는 디렉토리 안에 압축한 그대로 풀린다.
unzip filename.zip
```
