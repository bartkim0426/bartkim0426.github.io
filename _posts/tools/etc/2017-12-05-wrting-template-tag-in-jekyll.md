---
layout: post 
title:  "jekyll에서 django template tag 적기"
categories: jekyll
tags: jekyll blog
---

jekyll에 코드를 적는 중, 장고 템플릿태그를 jekyll이 ruby tag로 인식해버려서 다음과 같은 오류가 발생했다.

```bash
Liquid Exception: Liquid syntax error (line 9): Unknown tag 'verbatim'
```

escape 문자 `\` 등을 사용해서 해결해보려 했으나 잘 해결을 못하고 있던 와중에, raw 코드를 그대로 사용할 수 있는 태그가 있어서 이를 해결해봤다.  

```python
{% raw %}{% raw %}
{% verbatim %}
{{ contents }}
{% endverbatim %}
{% {% endraw %}endraw %}
```

다음과 같이 raw 태그를 사용하면 원하는 코드를 그대로 적을 수 있다.
