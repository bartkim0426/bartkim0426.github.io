---
layout: post 
title:  "Vue basic: from udemy(05~)"
categories: vue
tags: vue udemy
---


## 5강

### Instances
여러개의 vue 인스턴스 사용 가능: `#app1`, `#app2` 등으로... 각각의 인스턴스 변수명으로 서로 참조 가능하지만 한 기능은 한 인스턴스에서만 사용하는것이 좋음.


vue 코드 밖에서 `props`을 부여할 수 있지만, vue에서 이를 사용할 수 없음. ( `get`, `set`에서 인식 불가 )

```javascript
var vm1 = new Vue ({
	el: "#app1",
	data: {
		title: 'the vuejs instance',
		showParagraph: false
	},
	methods: {
		show: function() {
			this.showParagraph = true;
			this.updateTitle("the vuejs instance (updated)");
		},
		updateTitle: function(title) {
			this.title = title;
		}
	},
})

// 만들어진 vm1 vue instance 에 newProp이라는 prop을 추가 가능
vm1.newProp = 'New!';

// 콘솔로그를 찍어 보면 vm1.newProp을 확인 가능하다.
console.log(vm1);
```

### `$el`, `$data`
`console.log(vm1)`를 찍어보면 많은 기본 props들이 있는것을 볼 수 있음

- `$el`: html element가 뭔지 알려줌
- `$data`: vue instance에서 생성한 data. vue instance를 정의하기 전에 만들어 놓고 key, value 형태로 불러와도 됨.
```javascript
var data = {
	title: 'the vuejs instance',
	showParagraph: false,
}

var vm1 = new Vue ({
	el: "#app1",
	data: data,
	methods: {
		show: function() {
			this.showParagraph = true;
			this.updateTitle("the vuejs instance (updated)");
		},
		updateTitle: function(title) {
			this.title = title;
		}
	},
})

// console.log()를 찍어서 vue instance 내부의 data가 선언한 data와 같음을 확인
// true
console.log(vm1.$data === data)
```

### `$ref`
html instance들에 `ref` 속성을 준 후 js 내에서 `$ref`로 이를 접근 가능하다. (vue code 안 뿐 아니라 밖에서도 접근 가능)

하지만 이는 vue 속성이 아니라 dom에 바로 때리는거라서 change 될 수도 있다.

**html**
```html
<!-- heading, myButton ref를 선언 -->
<div id="app1">
	<h1 ref="heading">{{ title }}</h1>
	<button v-on:click="show" ref="myButton">Show Paragraph</button>
	<p v-if="showParagraph">This is not always visible</p>
</div>
```

**javascript**
```javascript
// vue instance 안에서 $ref로 접근 가능
var vm1 = new Vue ({
	el: "#app1",
	data: data,
	methods: {
		show: function() {
			this.$refs.myButton.innerText = 'test'
		},
		updateTitle: function(title) {
			this.title = title;
		}
	},
})

// vue insatance 밖에서도 접근이 가능하다.
vm1.$refs.heading.innerText = 'Something else'
```


### Mounting a template
```javascript
# el을 지정하지 않고 $mount()를 사용해도 됨
# el을 쉽게 지정할 수 있도록 만들어 놓은 것
var vm1 = new Vue ({
	// el: "#app1",
})

vm1.$mount('#app1');
```
> `$mount()` 등은 react 내부 함수. 이런식으로 불러서 사용 가능하다.

**temlpate**
html 안에 따로 내용을 넣지 않고 직접 `template`를 통해서 html string을 추가 가능하다.

```javascript
var vm3 = new Vue({
	template: '<h1>Hello</h1>'
});

vm3.$mount("#app3");
```


### Components
여러 번 재사용 할 수 있게 component로 만들어서 접근이 가능하다.
**html**
```html
<hello></hello>
<hello></hello>
```

**javascript**
```javascript
Vue.component(hello, {
	temlpate: <h1>Hello</h1>;
})
```

### Limitation of Template


### VueJs instance lifecycle

when create vue (`new Vue()`)
`beforeCreate()` => initialize data & event => `created()` => compile template => `beforeMount*()` => replace el with compiled template => mounted to DOM => data changed => `beforeUpdate()` => re-render DOM => `updated()` => `beforeDestroy()` => Destroyed => `destroyed()`

> 사진 첨부 (스크린샷)

### Lifecycle example
다음 예시를 따라 해 보면 vue instance lifecycle을 어느정도 이해하는데 도움이 된다.
**html**
```html
<div id="app">
	<h1>{{ title }}</h1>
	<button v-on:click="title = 'Changed'">Update title</button>
	<button v-on:click="destroy">Destroy</button>
</div>
```

**js**
```javascript
var vm = new Vue ({
	el: "#app",
	data: {
		title: "The vue instance",
	},
	beforeCreate: function() {
		console.log('beforeCreate()');
	},
	created: function() {
		console.log('created()');
	},
	beforeMount: function() {
		console.log('beforeMount()');
	},
	mounted: function() {
		console.log('mounted()');
	},
	beforeUpdate: function() {
		console.log('beforeUpdate()');
	},
	updated: function() {
		console.log('updated()');
	},
	beforeDestroy: function() {
		console.log('beforeDestroy()');
	},
	destroyed: function() {
		console.log('destroyed()');
	},
	methods: {
		destroy: function() {
			this.$destroy();
		}
	}
})
```

