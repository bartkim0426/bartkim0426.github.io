---
layout: post 
title:  "vue.js Dynamic component"
categories: vue
---


`<component></component>`를 사용해서 컴포넌트 파트를 동적으로 변화시킬 수 있다.

> 컴포넌트를 변경하면 매번 컴포넌트가 `Destory`되고 새로 생성된다.

### `App.js`
`App.js`에서 `selectedComponent`라는 데이터를 생성하고, 3개의 컴포넌트를 해당 데이터에 바인딩해주는 버튼을 생성.

`<component v-bind:is="selectedComponent">`를 이용해서 동적으로 템플릿에 뿌려주는 component를 변화시킬 수 있다.
```
<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12">
				<button v-on:click="selectedComponent = 'appQuote'">Quote</button>
				<button v-on:click="selectedComponent = 'appAuthor'">Author</button>
				<button v-on:click="selectedComponent = 'appNew'">New</button>
				<hr>
				<p>{{ selectedComponent }}</p>
				<component v-bind:is="selectedComponent">
					<p>Default content</p>
				</component>
				<!-- <app-quote> -->
				<!-- 	<h2 slot="title">{{ quoteTitle }}</h2> -->
				<!-- 	<p>Test</p> -->
				<!-- </app-quote> -->
            </div>
        </div>
    </div>
</template>

<script>
import Quote from './components/Quote.vue';
import appAuthor from './components/Author.vue';
import appNew from './components/New.vue';

    export default {
		data: function() {
			return {
				quoteTitle: 'The Quote',
				selectedComponent: 'appQuote'
			}
		},
		components: {
			appQuote: Quote,
			appAuthor,
			appNew
		}
    }
</script>
```

## dynamic component 동작원리
위에서 언급했듯이, dynamic component를 사용하여 다른 컴포넌트로 전환하면 항상 해당 컴포넌트는 destory되고, 새 컴포넌트가 create 된다. `destroyed` lifecycle로 이를 확인해 볼 수 있다.

> `couter` 데이터를 증가시키는 간단한 방법으로도 확인이 가능하다. 항상 `appNew` 컴포넌트가 될 때마다 매번 `counter`가 0이 되는 것 을 볼 수있다.



### `New.vue`
```javascript
<template>
	<div>
		<h3>New Quote</h3>
		<button v-on:click="counter++"></button>
		<p>{{ counter }}</p>
	</div>
</template>

<script>
export default {
	data: function() {
		return {
			counter: 0
		};
	},
	destroyed() {
		console.log('Destroyed')
	}
}
</script>
```

이제 `New` 버튼을 눌렀다가 다시 다른 버튼을 누르면 콘솔에 `Destroyed`가 찍히는 것을 볼 수 있다.


## Keep alive dynamic component
`<keep-alive></keep-alive>`를 사용하면 해당 컴포넌트가 destroy 되지 않고 계속 남아있다.

### `App.vue`
`<component></component>`를 `<keep-alive></keep-alive>` 태그로 감싸면 해당 컴포넌트가 계속 남아 있는 것을 볼 수 있다.  
```javascript
<template>
	<div class="container">
		<div class="row">
			<div class="col-xs-12">
				<button v-on:click="selectedComponent = 'appQuote'">Quote</button>
				<button v-on:click="selectedComponent = 'appAuthor'">Author</button>
				<button v-on:click="selectedComponent = 'appNew'">New</button>
				<hr>
				<p>{{ selectedComponent }}</p>
				<keep-alive>
					<component v-bind:is="selectedComponent">
						<p>Default content</p>
					</component>
				</keep-alive>
				<!-- <app-quote> -->
				<!-- 	<h2 slot="title">{{ quoteTitle }}</h2> -->
				<!-- 	<p>Test</p> -->
				<!-- </app-quote> -->
			</div>
		</div>
	</div>
</template>
```

> `keep-alive`를 쓴다면 `destory` lifecycle을 더 이상 사용할 수 없게 된다. 대신, `deactivated`, `activated` lifecycle을 사용할 수 있다.



