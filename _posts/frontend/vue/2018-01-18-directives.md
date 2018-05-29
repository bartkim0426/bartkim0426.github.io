---
layout: post 
title:  "vue.js custom Directives 만들기"
categories: vue
---


vue는 `v-for`, `v-text` 등 13가지 vue Directive들 이외에도, 커스텀 directive를 만들 수 있다는 큰 장점이 있다. 이는 여러 방면에서 유용하게 사용이 가능하다.

> vue의 기본 directive들은 [이 글]()에서 다루었다.


## vue directive 만들기
vue directive는 `main.js`에서 전역으로 만든다. 

`Vue.directive();`로 등록이 가능하다.
### `main.js`에서
```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.directive('highlight', {});

new Vue({
  el: '#app',
  render: h => h(App)
}
```

## hooks
위의 directive 정의 객체는 5가지 훅 함수를 제공한다. 어떤 상황에서 사용할 것인지를 결정해준다.
- `bind`: `bind(el, binding, vnode)` => Once directive is attached (처음 attach 되었을때만) ==> 주로 많이 사용됨
- `inserted`: `insert(el, binding, vnode)` => Inserted in parent node (parent node에 추가되었을때)
- `update`: `update(el, binding, vnode, oldVnode)` => Once component is updated (without children). 
- `componentUpdated`: `componentUpdated(el, binding,vnode, oldVnode)` => Once component is updated (with childern)
- `unbind` => once directive is removed (제거되었을때)


## making simple custom directive
bgcolor를 green으로 바꾸는 아주 심플한 directive를 만들어 보겠다.

## `el` attribute
`el` attribute는 directive가 접근할 엘리멘트/컴포넌트의 파트를 지정할 때 쓰인다. 브라우저의 `element`와 동일.
```javascript
Vue.directive('highlight', {
	bind(el, binding, vnode) {
		el.style.backgroundColor = 'green';
	}
});
```

이제 컴포넌트들에서 `v-highlight` 디렉티브를 넣으면 bgcolor가 green이 되는 것을 볼 수 있다.
```javascript
<p v-highlight></p>
```

## `binding`
`binding` attribute는 directive를 사용한 엘리멘트/컴포넌트의 바인딩 된 데이터를 받는다.

```javascript
Vue.directive('highlight', {
	bind(el, binding, vnode) {
		el.style.backgroundColor = binding.value;
	}
})
```

```html
<p v-highlight="'red'"></p>
```

## Passing arguments
argument를 directive에 주어서 argument에 따라 다른 결과를 얻을 수 있다.
```javascript
Vue.directive('highlight', {
	bind(el, binding, vnode) {
		if (binding.arg == 'background') {
			el.style.backgroundColor = binding.value;
		} else {
			el.style.color = binding.value;
		}
	}
})
```

```html
<p v-highlight:background="'red'"></p>
<p v-highlight="'red'"></p>
```

## making Modifiers in directive
`v-highlight:background.delayed`의 `.delayed`와 같은 modifier를 직접 추가할 수 있다.

추가한 modifier는 `binding.modifiers` 객체로 directive 코드에서 불러올 수 있다.

### `main.js`에서..
```javascript
Vue.directive('highlight', {
	bind(el, binding, vnode) {
		var delay = 0;
		if (binding.modifiers['delayed']) {
			delay = 3000;
		}
		setTimeout(() => {
			if (binding.arg == 'background') {
				el.style.backgroundColor = binding.value;
			} else {
				el.style.color = binding.value;
			}
		}, delay)
	}
})
```

```html
<p v-highlight.delayed="'red'">Red</p>
```


## Register local directive
directive를 gloal이 아니라 local로 등록할 수도 있다. `components`를 추가할 때와 동일하게 `directives`로 추가해주면 됨.

```javascript
<script>
	export default {
		directives: {
			'local-highlight': {
				bind(el, binding, vnode) {
					// el.style.backgroundColor = 'green';
					// el.style.backgroundColor = binding.value;
					var delay = 0;
					if (binding.modifiers['delayed']) {
						delay = 3000;
					}
					setTimeout(() => {
						if (binding.arg == 'background') {
							el.style.backgroundColor = binding.value
						} else {
							el.style.color = binding.value
						}
					}, delay);
				}
			}
		}
	}
</script>
```

## Multiple modifiers
modifier를 여러개 추가할 수도 있다. 색상을 1000ms 간격으로 바꿔주는 `.blink`라는 modifier를 만들어서 추가해보자.

```javascript
<script>
	export default {
		directives: {
			'local-highlight': {
				bind(el, binding, vnode) {
					// el.style.backgroundColor = 'green';
					// el.style.backgroundColor = binding.value;
					var delay = 0;
					if (binding.modifiers['delayed']) {
						delay = 3000;
					}
					if (binding.modifiers['blink']) {
						let mainColor = binding.value;
						let secondColor = 'blue';
						let currentColor = mainColor;
						setTimeout(() => {
							setInterval(() => {
								currentColor == secondColor ? currentColor = mainColor : currentColor = secondColor;
								if (binding.arg == 'background') {
									el.style.backgroundColor = currentColor
								} else {
									el.style.color = currentColor
								}
							}, 1000);
						}, delay)
					} else {
						setTimeout(() => {
							if (binding.arg == 'background') {
								el.style.backgroundColor = binding.value
							} else {
								el.style.color = binding.value
							}
						}, delay);
					}
				}
			}
		}
	}
</script>
```

```html
<p v-local-highlight:background.delayed.blink="'red'">Color</p>
```

## Passing complex value to directive
value를 object 형태로 넘겨서 `binding.value.mainColor` 형식으로 사용이 가능하다.

```html
<p v-local-highlight:background.delayed.blink="{mainColor: 'red', secondColor: 'green', interval: 500}"></p>
```



