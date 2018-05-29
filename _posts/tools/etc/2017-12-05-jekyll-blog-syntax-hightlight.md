---
layout: post 
title:  "Jekyll blog 에서 syntax hightlight 사용-kramdown, rouge"
categories: etc
tags: jekyll github blog
---

jekyll 에서는 기본 syntax highlight를 지원해준다. 다음과 같이 pygments를 사용하면 된다.

```ruby
{% raw %}
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}
{% endraw %}
```

문제 없이 highlighting이 잘 되지만, 직관적이지 않고 쓰기가 조금 불편한 면이 있는 것 같아서 grave를 사용한 syntax highlighting을 할 수 있는 방법을 찾아보았다.

그러던 중 `Rouge`를 발견하여 사용해보았는데, 사용법도 편리하고 괜찮은 듯 해서 설치법을 공유한다.


> Jekyll 2.5.0 이상 버전에만 사용 가능하다. `jekyll -v`로 버전을 확인 가능


일단 `kramdown`과 `rouge`를 설치해준다.
```ruby
gem install kramdown rouge
```

설치가 다 되었따면 `_config.yml` 파일에 설정을 추가해준다.
```
markdown: kramdown

kramdown:
	input: GFM
	syntax_hightlighter: rouge
```

이후 많은 마크다운 에디터(typora) 등에서 사용하는 방식으로 syntax highlight를 사용 가능하다.
```html
{% raw %}
```html
<a href="">Hello World</a>
``\`
{% endraw %}
```


> github page에서 사용하려면 더 간편하게 다음 두 줄만 추가하면 된다.
```
markdown: kramdown
highlighter: rouge
```


이 외에도 [ `redcarpet` ](https://george-hawkins.github.io/basic-gfm-jekyll/redcarpet-extensions.html) 등도 있으니 입맞에 맞게 사용하면 될 듯 하다.
