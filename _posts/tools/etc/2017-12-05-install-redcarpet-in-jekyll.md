---
layout: post
title: "jekyll syntax highlighting - redcarpet"
categories: jekyll
tags: jekyll redcarpet
---

`rouge`를 syntax highlighting 도구로 잘 사용하고 있다가, 이를 tistory blog로 복사하면 code block 안의 글자들이 하얀 색으로 나타나는 증상이 발생하였따. 그래서 이번 기회에 `redcarpet`이라는 다른 하이라이팅 도구를 사용해보았다.


### 설치
```ruby
sudo gem install redcarpet
```

### config
```
# Conversion markdown: redcarpet 
highlighter: pygments 
redcarpet: extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "strikethrough", "superscript"]
```
