---
layout: post
title:  "django: reversed forloop in template"
categories: django
---

장고 템플릿에서 if와 함께 가장 많이 사용하는 템플릿태그는 for이다. 그냥 단순히 객체를 순회하는 것 말고도 많은 기능들이 있는데, 이번에는 파이썬에서 `enumerate`와 비슷한 기능을 해주는 `forloop`를 소개한다.


for문 안에서 `forloop.counter`를 사용하면 for문이 순회하는 순서를 1부터 시작해서 사용할 수 있다. 0부터 사용하려면 `forloop.counter0`을 사용하면 된다.

{% raw %}
```
{% for post in posts %}
{{ forloop.counter }}번째 글: {{ post.id }} - {{ post.name }}
{% endfor %}
```
{% endraw %}

만약 이 `counter`를 거꾸로 사용하고 싶다면? `forloop.revcounter`를 사용하면 된다!
{% raw %}
```
{% for post in posts %}
{{ forloop.revcounter }}번째 글: {{ post.id }} - {{ post.name }}
{% endfor %}
```
{% endraw %}


또한 순회하는 리스트 자체를 뒤에서부터 순회하는 `reversed` 옵션도 있다.
{% raw %}
```
{% for post in posts reversed %}
{{ forloop.counter }}번째 글: {{ post.id }} - {{ post.name }}
{% endfor %}
```
{% endraw %}


장고는 기본 템플릿태그 등에 상당히 많은 공을 들여 놓아서 잘만 찾아보면 많은 기능들을 유용하게 사용할 수 있다.
