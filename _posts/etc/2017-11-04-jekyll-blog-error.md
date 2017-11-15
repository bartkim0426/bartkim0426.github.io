---
layout: post
title:  "Jekyll blog 오류 발생시 해결"
categories: jekyll ruby blog
---

## `bundle exec jekyll serve` 오류 발생
`Bundler could not find compatible versions for gem "jekyll"` 오류가 발생... 검색 후 다음과 같이 해결

1. Gemfile, Gemfile.lock을 삭제
2.  Gemfile에 다음을 입력
```
source 'https://rubygems.org'
gem 'github-pages'
```
3. `bundle install`
