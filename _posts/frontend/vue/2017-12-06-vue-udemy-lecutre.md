---
layout: post 
title:  "Vue basic: from udemy (01~04)"
categories: vue
tags: vue udemy
---


## 2강

### Directives 이해, `v-once`

```javascript
new Vue ({
	el: "#app",
	data: {
		title: "Hello world",
		link: "https://seulcode.tistory.com",
	},
	methods: {
		sayHello: function() {
			this.title = "Hello";
			return this.title;
		},
	},
})
```
다음 코드에서 `sayHello()`를 호출하면 기존의 `Hello world`가 `hello`로 대체된다. 이를 막기 위해서는 `v-once`를 통해 re-rendering을 방지 가능하다.

### Raw html 표시하기: `v-html`

vue에서 다음과 같이 finishedLink에 html을 넣고 html에서 double curly로 불러오면 string으로 불러진다. 이를 html처럼 사용하고 싶다면 `v-html=""` 이런식으로 사용 가능.
```javascript
new Vue ({
	el: "#app",
	data: {
		finishedLink: '<a href="http://www.google.com">Google</a>',
	}
})

```

```html
{% raw %}
<body>
	<div id="app">
		<p>{{ finishedLink }}</p>
		<p v-html="finishedLink"></p>
	</div>
</body>
{% endraw %}
```

### Event: `v-on`
event를 받아들이기 위해서 vue는 `v-on`을 사용한다.

```html
	<div id="app">
		<button v-on:click="increaseClick(2, $event)">Click me</button>
		<p>{{ counter }}</p>
	</div>
```

```javascript
new Vue ({
	el: "#app",
	data: {
		counter: 0,
	},
	methods: {
		increaseClick: function(num, event) {
			this.counter += num;
		}
	}
})
```

### Event: `stopPropagation`, or stop
```html
<span v-on:mousemove.stop>DEAD SPOT</span>
<span v-on:mousemove.stop.prevent>DEAD SPOT</span>

<!-- 이런 방식으로도 사용 가능함 -->
<span v-on:mousemove="dummy">DEAD SPOT</span>

<!-- js에서 stopPropagation -->
new Vue ({
...
		dummy: function() {
			event.stopPropagation();
		},
...
})
```

공식 문서 중 [여기](https://vuejs.org/v2/guide/events.html#Event-Modifiers)에서 모든 event modifiers를 확인할 수 있다. 대표적인 것들로는

- `.stop`
- `.prevent`
- `.once`
등이 있다. 

### Event: keyup modifiers
vue에서 `v-on:keyup`으로 keyup event를 인식할 수 있다. `KeyCode`를 사용해서 keyup 이벤트를 인식할 수 있는데, keycode가 직관적이지 못해서 그런지 vue에서 자주 쓰이는 키들에 대해서는 alias를 제공한다. 대표적으로
- enter
- esc
- delete
- space
- up
- down

```html
<input type="" v-bind:keyup.13="submit">
<!-- 13과 동이한 enter를 입력해도 됨 -->
<input type="" v-bind:keyup.enter="submit">
```

뿐만 아니라 본인의 alias도 만들 수 있다.
```
//  이후부터는 f1을 사용 가능하다.
Vue.config.keyCodes.f1 = 112
```

### Vue template 문법
vue template 내에는 한 expression javascript를 사용할 수 있다.
```html
{% raw %}
<button v-on:click="increaseClick(2, $event)">Click me</button>
	<p>{{ counter }}</p>
<!-- 위 코드를 다음과 같이 사용 가능하다. -->
<button v-on:click="counter++">Click me</button>
	<p>{{ counter > 10 ? "Greater than 10" : counter }}</p>
{% endraw %}
```


### Two way binding: `v-model`
`v-model`을 사용해서 vue object의 data 값을 양방향으로 binding 가능

```html
<div id="app-23">
	<input type="" v-model="name">
	<p>{{ name  }}</p>
</div>
```

### Computed properties
- method: cache 되지 않고 매번 update 됨
- computed: cache된 값을 반환. 
상황에 맞게 사용하면 된다.

### Watch
 `watch`를 사용해서 counter를 2초마다 0으로 만들수있음

 ```javascript
	watch: {
		counter: function(value) {
			var vm = this;
			setTimeout(function() {
				vm.counter = 0;
			}, 2000);
		}
	},
 ```

### shrotcuts for `v-on`, `v-bind`
`v-on:`은 `@`, `v-bind` 는 `:`로 사용 가능하다.
```html
<button v-on:click="changeLink">Change link</button>
<a v-bind:href="link">{{ link }}</a>
<!--위의 코드와 동일 -->
<button @click="changeLink">Change link</button>
<a :href="link">{{ link }}</a>
```

### Using wtih css
css를 vue에서 binding하려면 `v-bind:class`를 사용하면 됨

```css
.demo {
	width: 100px;
	height: 100px;
	background-color: gray;
	display: inline-block;
	margin: 10px;
}

.red {
	background-color: red;
}

.green {
	background-color: green;
}

.blue {
	background-color: blue;
}
```

```html
	<div id="app">
		<div class="demo"
		v-on:click="changeRed = !changeRed"
		<!-- css class를 바로 넣어줘도 되고, { css-class, condition }의 js object를 넣어줘도 됨 -->
		v-bind:class="{ red : changeRed, blue !changeRed}" ></div>

		<!-- divClassess라는 js object를 vue 안에 computed 시켜서 불러올 수 있음 -->
		<div class="demo"
		v-bind:class="divClasses"></div>

		<!-- v-model로 color를 binding 시켜 놓고  -->
		<div class="demo"
		<!-- class 안에는 class나 class를 key로 가진 object를 넣어도 되고, classname의 list를 넣어도 됨.
		v-bind:class="[color, {red: changeRed}]"></div>
		<input type="text" v-model="color">
	</div>
```

```javascript
new Vue({
	el: "#app",
	data: {
		chageRed: false
	},
	// computed로 vue object 내에 class 선언을 넣어줌
	computed: {
	divClasses: function() {
		return {
			red: this.changeRed,
			blue: !this.changeRed
		}
	},
	}
})
```

### Css style 이용하기
`v-bind:style=""`으로 style을 직접 사용 가능하다. 주의해야 할 점은 `background-color` 같이 non-char가 나오면 camelcase (backgroundColor)로 적어주어야 한다는 것! 직접 html에 적을수도 있고, computed 등을 사용해서 js object로 불러와도 무방하다.

**html**
```html
		<!-- 이렇게 style에 직접 css 추가가 가능하고, -->
		<div class="demo" 
			v-bind:style="{backgroundColor: color}"></div>
		<!-- myStyle같은 js object를 불러와서 사용 가능하다.-->
		<div class="demo"
			v-bind:style="myStyle"></div>
		<!-- 다음과같이 array를 사용해서 object와 css를 동시에 사용도 가능하다.-->
		<div class="demo"
			 v-bind:style="[myStyle, {height: width + 'px'}]"></div>
```

**js**
```javascript
var vm = new Vue({
	el: "#app",
	data: {
		color: "gray",
		width: 100,
	},
	computed: {
		myStyle: function() {
			return {
				backgroundColor:  this.color,
				width: this.width + "px",
			};
		}
	}
})
```

## 3. Conditions & Rendering list

### `v-if`
`v-if`를 사용하여 특정 html element에 조건을 부여할 수 있다.

```html
	<div id="app">
		<p v-if="show">You can see me! <span>Hello</span></p>
		<p v-else>See me?</p>
		<button v-on:click="show = !show">switch</button>
	</div>
```

```javascript
new Vue ({
	el: "#app",
	data: {
		show: true,
	}
})
```

> vue 2.1 이상 버전을 사용한다면 `v-else-if`도 사용 가능하다.

### html5 - `template`
`v-if` 등은 모든 하위 엘리멘트에 적용된다. 만약 여러 엘리멘트에 `v-if`를 적용하고 싶은데, div나 span 등을 사용하고 싶지 않다면 `template` 태그를 사용하면 된다. 이는 하위 엘리멘트들을 묶어주는 효과가 있지만 실제 브라우저에서는 나오지 않는다.

```html
<template v-if="show">
	<h1>Heading</h1>
	<p>Inside a template</p>
</template>
```
### `v-show`
`v-if`, `v-else`를 사용하면 `show=false`가 되었을 때 해당 엘리먼트가 deattached된다. dom은 엘리멘트가 적은게 좋기 때문에 이런식으로 deattach 하는것이 좋지만, 특수한 상황에서 단지 `display=none`을 적용하여 가리고 싶다면 `v-show`를 사용 가능.

```html
<p v-show="show">Seeing??</p>
```

이제 `show=false`일 경우, `display=none`이 적용되어 브라우저 상에서는 보이지 않지만 실제 요소에는 남아있는 것을 볼 수 있따.



### `v-for`
`v-for`를 사용해서 loop를 돌릴 수 있다. `v-for="(item, index) in items"`로 해당 item의 index도 접근 가능.

```html
<div id="app">
	<ul>
		<li v-for="( ingredient, i ) in ingredients">{{ ingredient }} ({{ i }})</li>
	</ul>
</div>
```

```javascript
new Vue ({
	el: '#app',
	data: {
		ingredients: ['meat', 'fruit', 'cookies'],
		persons: [
			{name: 'Max', age: 26, color: 'red'},
			{name: 'Anna', age: 'unknown', color: 'blue'},
		],
	}
})
```

### `v-for`에서 `template` 사용하기
다음과 같이 써서 위의 `v-for` 예시와 비슷하게 template 내에  for looping 된 값들을 넣어줄 수 있다.
```html
<temlpate v-for="( ingredient, index ) in ingredients">
	<h1>{{ ingredient }}</h1>
	<p>{{ index }}</p>
</temlpate>
```

### `v-for`로 `key`, `value` 표시하기, nested v-for
`v-for="(value, key, index)"`의 순서로 value, key, index에 각각 접근이 가능하다 		

또한 v-for를 중첩해서 하나의 object를 `v-for`로 순회하면서 사용 가능

```html
<ul>
	<li v-for="person in persons">
		<div v-for="(value, key, index) in person">{{ key }}-{{ value }} ({{ index }})</div>
	</li>
</ul>
```

### loop numbers
다음과 같이 숫자를 looping 할 수도 있따.
```html
<span v-for="n in 10">
	{{ n }}
</span>
```

### 안전한 `v-for` 사용을 위해서 `key` 추가하기
vue 공식문서에서도 언급하듯이, 각각의 for looping 된 아이템들에 unique한 key를 추가해주는 것이 좋다.

보통은 index나 id 값 등을 이용한다. (unique한 속성) 

```html
<ul>
	<li 
		v-for="( ingredient, i ) in ingredients"
		v-bind:key="ingredient">{{ ingredient }} ({{ i }})</li>
</ul>
```
