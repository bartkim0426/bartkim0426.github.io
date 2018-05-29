---
layout: post 
title:  "django, vue double curly in template"
categories: vue
tags: vue django
---

django 프로젝트를 하면서 vue를 사용할 일이 있어서 함께 사용해보려고 하다가 template 에서 double curly를 사용하지 못하게 되서 찾아보았다. 장고에서도 double curly를 context에 사용하고, vue에서도 double curly를 data binding에서 사용하기 때문에 이를 해결해주어야 한다.


## `verbatim` template tage 사용
django 빌트인 템플릿태그 중 `verbatim`을 사용하면 잠시 템플릿 엔진을 멈추게 해서 double curly를 스트링으로 사용 가능하다.

```html
<div id="app">
	{% raw %}
	{% verbatim %}
	{{ contents }}
	{% endverbatim %}
	{% endraw %}

</div>

<script type="text/javascript">
	var app = new Vue({
		el: "#app",
		data: {
			contents: "hello"
		}
	})
</script>
```
이제 content의 데이터로 "hello" 가 출력되는 것을 볼 수 있다.

## vue의 `delimiter` 변경하기
vue의 기본 delimiter인 double curly를 `{  }`나 `[[  ]]`로 바꾸면 장고 템플릿과의 충돌 없이 사용 가능하다.
```html
<div id="app">
	{ contents }
</div>

<script type="text/javascript">
	var app = new Vue({
		delimiters: ['{',  '}'],
		el: "#app",
		data: {
			contents: ""
		}
	})
</script>
```

## `{`, `}` 대신 unicode character 사용
`{`에 해당하는 `&#123`, `}`에 해당하는 `&#125`를 사용하면 정상적으로 출력되는 것을 확인할 수 있다.
```
<p>"&#123;&#123; some text &#125;&#125;"</p>
```
