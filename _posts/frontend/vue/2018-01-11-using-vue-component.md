---
layout: post 
title:  "vue 컴포넌트 사용법 (using vue components)"
categories: vue
---

# Vue Components 사용법


## 기초 세팅 (`main.js`, `App.vue`)
이번 component 예제에는 webpack을 사용하였다. 자세한 내용은 [vue webpack]()에서 다룰 예정. 

`src` 디렉토리에 `main.js`와 `App.vue`를 만들고, `main.js`에 `App.vue`를 등록해 주어야 한다.

### `index.html`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Vue Components</title>
  </head>
  <body>
    <div id="app">
    </div>
    <script src="/dist/build.js"></script>
  </body>
</html>
```

### `main.js`
```
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})
```

### Vue components 만들기
vue component는 `template`, `script`, `style` 크게 세가지로 구성된다.

다음은 vue component의 예시. `App.vue`에 다음과 같이 입력한다.

### `App.vue`
```html
<template>
	<div>
		<h1>{{ title }}</h1>
	</div>
</template>

<script>
export default {
	data: function() {
		  return {
			  title: "Hello World"
		  }
	}
}
</script>

<style>
</style>
```

이제 브라우저에서 `Hello World`가 나오는 것을 볼 수 있다. `App.vue`라는 하나의 컴포넌트를 main.js에서 뿌려준 것.


여러 개의 컴포넌트를 사용하려면 component를 `App.vue` 내에서 등록하거나, `main.js`에서 글로벌로 등록 해 주어야 한다.

우선 새로운 component를 만들어보자. `data`로 `content`를 가지고 있는 간단한 component를 만들어 보겠다.

### `Content.vue`
```html
<template>
	<p>{{ content }}</p>
</template>

<script>
export default {
	data: function() {
		  return {
			  content: "this is test content"
		  }
	}
}
</script>
```

## Global
이제 위에서 만든 `Content.vue` 컴포넌트를 호출해야 한다. 먼저 global로 호출해서 전역에서 사용 할 수 있게 등록하는 방법이다.

### `main.js`
```javascript
import Vue from 'vue'
import App from './App.vue'
// 우선 만들어 놓은 Content.vue를 호출
import Content from './Content.vue'

// globally register component
// 이름은 맘대로 해도 상관 없다. 나는 app-content로 등록
Vue.component('app-content', Content);

new Vue({
  el: '#app',
  render: h => h(App)
})
```

등록한 컴포넌트를 `App.vue`에서 호출하여 사용 가능하다.
### `App.vue`
```html
<template>
	<div>
		<h1>{{ title }}</h1>
		<app-content></app-content>
	</div>
</template>

<script>
export default {
	data: function() {
		  return {
			  title: "Hello World"
		  }
	}
}
</script>

<style>
</style>
```

이제 title 밑에 위에서 적은 content가 정상적으로 호출되는 것을 볼 수 있다.

## App 내에서 불러서 사용
전역으로 사용하지 않고, 해당 앱 내에서 사용할 경우 해당 컴포넌트를 앱 내에서 호출 가능하다.

`main.js`는 기존 상태 그대로 둔 후,
### `main.js`
```javascript
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})
```

`App.vue`에서 `Content.vue`를 호출한다. `<script></script>` 내부에서 호출한 후 이를 `components`에 등록 해주면 된다.
### `App.vue`
```html
<template>
	<div>
		<h1>{{ title }}</h1>
		<app-content></app-content>
	</div>
</template>

<script>
import Content from './Content.vue';

export default {
	components: {
		'app-content': Content
	},
	data: function() {
		  return {
			  title: "Hello World"
		  }
	}
}
</script>

<style>
</style>
```

아까와 동일하게 호출되는 것을 볼 수 있다.


### 컴포넌트 호출 시 이름 확장


위에서 `Content` 컴포넌트를 `app-content`로 호출했는데, 이를 CamelCase로 기본으로 호출하면 동일하게 사용할 수 있다.


즉, `"app-content"` 대신에 `appContent`로 호출하면, `<appContent></appContent>`로도 사용할 수 있지만, 
`<app-content></app-content>`로도 사용이 가능하다.

```javascript
<template>
	<div>
		<h1>{{ title }}</h1>
		<app-content></app-content>
	</div>
</template>

<script>
import Content from './Content.vue';

export default {
	components: {
		appContent: Content
	},
	data: function() {
		  return {
			  title: "Hello World"
		  }
	}
}
</script>

<style>
</style>
```

뿐만 아니라 vue는 호출한 component 이름과 동일하게 호출 할 경우 축약할 수 있는 기능을 제공 해준다.

`Content`라는 컴포넌트를 호출했기 때문에, 아래 예시처럼 `components`에 `Content`만을 적어주면, template에서 `content`로 바로 사용이 가능하다.

> 이런 방식을 사용할 경우, `<app-`로 시작하는 하이픈을 사용할 수 없기 때문에 여러 앱을 동시에 사용해야 하는 경우에는 추천하지 않는다.

```javascript
<template>
	<div>
		<h1>{{ title }}</h1>
		<content></content>
	</div>
</template>

<script>
import Content from './Content.vue';

export default {
	components: {
		Content
		// this is same as
		// Content: Content
	},
	data: function() {
		  return {
			  title: "Hello World"
		  }
	}
}
</script>
```


## Style 사용하기

기본적으로, 어떤 컴포넌트에 있는 스타일이라도 global로 적용이 된다. 

이를 특정 컴포넌트에만 적용하려면 `scoped`를 추가해주어야 한다. 

실제로 만들어진 html을 보면 `data-`로 시작되는 attribute가 만들어져 거기에 스타일이 추가됨.
```javascript
...
<style scoped>
div {
	border: solid 1px red;
}
</style>
```


## Component간의 데이터 전달
`UserDetail`을 가지고 있는 `User.vue` 파일에서 user의 name을 detail component로 전달하고 싶다면, `props`를 사용하면 된다.

리액트를 사용해 본 적이 있다면, `props`에 익숙 할 것이다. 상위 컴포넌트에서는 `v-bind:`를 사용하여 prop을 넘겨 주어야 하고, 하위 컴포넌트에서는 `props` 옵션을 써서 받는 props를 명시해주어야 하는 점이 react와 조금 다르다.


아래 예시에서는 `UserDetail`에 `v-bind` 옵션을 사용해서 `myName`이라는 props 전달 이름으로 `name` 값을 전달한다.

> `v-on:click="changeName"` 메쏘드는 전달된 이름이 잘 바뀌는지 확인하기 위해 넣음.


### `User.vue`
```javascript
<template>
<div>
	<app-user-detail v-bind:myName="name"></app-user-detail>
	<button v-on:click="changeName">Change my name</button>

</div>
</template>

<script>
import UserDetail from './UserDetail.vue';

export default {
	data: function() {
		return {
			name: "Seul"
		}
	},
	methods: {
		changeName() {
			this.name = "Kim";
		}
	},
	components: {
		appUserDetail: UserDetail,
	}
}
</script>
```

### `UserDetail.vue`
자식 컴포넌트인 `UserDetail`에서는 `props`로 받을 값을 명시적으로 적어 주어야 한다. (`myName`) 이를 적은 후에는 정상적으로 부모 컴포넌트의 값을 가져다가 쓸 수 있다.

```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Name: {{ myName }}</p>
    </div>
</template>

<script>
export default {
	props: ['myName']
}
</script>
```

> 싱글 페이지 어플리케이션이 아닌 경우에?는 html에서 `myName`(카멜케이스)를 사용할 수 없기 때문에 하이픈(-)으로 구분되는 `kebab-case`를 사용해야한다. 위의 예시를 보면 `myName` 대신 `my-name`을 사용하면 된다.
```javascript
...
<app-user-detail v-bind:my-name></app-user-detail>
...
```

### Props validation
부모 컴포넌트로부터 정해진 데이터 타입 (`String`, `Array` 등..)이 들어와야 하거나 필수적으로 들어와야 하는 값 등이 있다면 validation을 해 줄 필요가 있다.

그럴 경우 props 객체를 object로 생성하고 그 안에 필요한 값을 넣어주면 된다.
```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Name: {{ myName }}</p>
    </div>
</template>

<script>
export default {
	props: {
		myName: String
		// 만약 multiple type을 지원한다면
		// myName: [String, Array]
	},
	methods:
		reverseName() {
			this.myName.split("").reverse().join("")
		}
}
</script>
```

꼭 필요한 props가 있다면 `required`를 적용시키면 된다. 이럴 경우 `myName` (props)도 object로 작성해준다.

```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Name: {{ myName }}</p>
    </div>
</template>

<script>
export default {
	props: {
		myName: {
			type: String,
			required: true
			// 다음과 같이 default 값을 줄 수도 있다.
			// default: "John"
		}
	// object를 사용하는 경우
	// 		myName: {
	// 			type: Object,
	// 			default: function() {
	// 				return { ... }
	// 			}
	// 		}
	},
	methods:
		reverseName() {
			this.myName.split("").reverse().join("")
		}
}
</script>
```

## props를 바꾸고 싶을 때?
`vue.js`의 데이터 흐름은 `단방향` 이기 때문에 부모 컴포넌트로부터 받은 props를 수정하는 것은 불가능 하다.


### methods로 props 수정하기
하지만 모든 props는 뷰 안에서 하나의 자바스크립트 객체로 사용 가능하기 때문에, 수정/변경이 필요할 경우 `methods`를 사용해서 변경이 가능하다.

다음 예제는 이전 예시에서 가져온 `myName`을 reverse 시키는 함수를 보여준다.
{% raw %}
```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Name: {{ reverseName() }}</p>
    </div>
</template>

<script>
export default {
	props: ['myName'],
	methods:
		reverseName() {
			this.myName.split("").reverse().join("")
		}
}
</script>
```
{% endraw %}

화면에 기존 이름이 reverse 되어 표시되는 것을 볼 수 있다. 이처럼 부모로부터 받은 props 자체를 수정해서 부모 컴포넌트에 영향을 끼치는 것은 불가능하지만, 자식 컴포넌트 안에서 methods를 이용해서 해당 props를 자유롭게 사용 가능하다.

### emit the event - `event`를 사용하여 부모 컴포넌트에 전달
위에서 언급했듯이 자식 컴포넌트 안에서 props를 수정하면 자식 컴포넌트에서만 적용되고, 부모 컴포넌트로 전달되지 않는다.

대신 부모 컴포넌트에게 props 값이 변경되었음을 알리는 방법을 사용할 수 있는데, 이 때 `event`를 전달한다. 

> vue.js 공식 문서에서 나온 "props는 아래로, events는 위로"가 이런 뜻에서 사용된 말이라고 생각한다.

이 때 event를 전달하기 위해서 `$emit(eventName)`을 사용한다. 

아래 예시는 `resetName`을 클릭하면 `nameWasReset`이라는 이벤트를 부모 컴포넌트로 전달시키는 예시이다.

### `UserDetail.vue`
{% raw %}
```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Name: {{ reverseName() }}</p>
		<button v-on:click="resetName">Reset name</button>
    </div>
</template>

<script>
export default {
	props: ['myName'],
	methods:
		reverseName() {
			this.myName.split("").reverse().join("")
		},
		resetName() {
			this.myName = "Max";
			this.$emit('nameWasReset', myName);
		}
}
</script>
```
{% endraw %}

### `User.vue`
부모 컴포넌트인 `User.vue`에서는 `v-on`을 사용하여 `event`를 감지하도록 템플릿에 지정해야한다.
```script
...
<app-user-detail 
	v-bind:my-name="name" 
	v-on:nameWasReset="name = $event">
</app-user-detail>
...
```

## Component간의 data flow

component끼리의 data 흐름에서 기억해야 할 것은, vue는 `Unidirectional Data flow`를 가진다는 것이다.

기본적으로 자식 컴포넌트끼리 데이터 교환은 불가능하고, 부모 컴포넌트를 통해서 데이터를 전달해 주어야 한다.

이를 가능하게 해주는 방법은 크게 세가지.

1. `$emit`을 사용하여 부모 컴포넌트의 data 변경

2. `callback` 사용

3. (추천하는 방법??) `Event Bus`
## Event Bus
부모 컴포넌트를 통하는 게 아닌, `main.js`에 등록한 `eventBus`를 통해서 `$emit`, `$on`으로 자식 컴포넌트의 데이터를 전달해주는 방법.

### `main.js`
`main.js`에 `EventBus`를 등록해준다. 

이 때 `#app`에 등록할 Vue instance를 선언하기 전에 `eventBust`를 등록해 주어야 한다.
```javascript
import Vue from 'vue'
import App from './App.vue'

export const eventBus = new Vue();

new Vue({
  el: '#app',
  render: h => h(App)
})
```

### `UserEdit.vue`
이후 age의 값을 변경시킬 `UserEdit.vue`에서 `eventBus`를 호출한 후, `eventBus`에 age가 변했다는 사실을 전달시켜준다.

이때 전달은 `vue lifecycle`에서 `created()`를 사용한다.
```javascript
<template>
    <div class="component">
        <h3>You may edit the User here</h3>
        <p>Edit me!</p>
		<p>User Age: {{ userAge }}</p>
		<button v-on:click="editAge">Edit age</button>
    </div>
</template>

<script>
import { eventBus } from '../main';
export default {
	props: ['userAge'],
	methods: {
		editAge() {
			this.userAge = 30;
			eventBus.$emit('ageWasEdited', this.userAge);
		}
	}
}
</script>
```

### `UserDetail.vue`
그 다음 위에서 변화된 값을 받아서 변경시킬 vue 파일 (`UserDetail.vue`)에서 `$on`을 이용하여 값을 받은 뒤 전달시켜준다.
```javascript
<template>
    <div class="component">
        <h3>You may view the User Details here</h3>
        <p>Many Details</p>
		<p>User Age: {{ userAge }}</p>
    </div>
</template>

<script>
import { eventBus } from '../main.js';
export default {
	props: {
		userage: number
	},
	created() {
		eventBus.$on('ageWasEdited', (age) => {
			this.userAge = age;
		};
	}
}
</script>
```


## `eventBus` 자체에 age를 변경시키는 methods 추가하기
위의 예제에서 `eventBus`는 그저 데이터를 emit, on 시켜주기만 할 뿐 다른 기능은 하나도 없었따.

이렇게 하지 않고, `eventBus` 자체에 data나 methods를 추가할 수 있다.

### `main.js`
우선 `main.js`의 `eventBus`에 위의 예제에서 사용한 age를 `$emti`하는 method를 추가해준다.
```javascript
import Vue from 'vue'
import App from './App.vue'

export const eventBus = new Vue({
	methods: {
		changeAge(age): {
			this.$emit('ageWasEdited', age);
		}
	}
});

new Vue({
  el: '#app',
  render: h => h(App)
})
```

### `UserEdit.vue`
이후 `UserEdit.vue`에 있던 `$emit` 부분을 위에서 만든 `eventBus`의 method로 대체한다.
```javascript
...
methods: {
	editAge() {
		this.userAge = 30;
		// 기존에 있던 부분
		// eventBus.$emit('ageWasEdited', this.userAge);
		// eventBus에서 가져오기
		eventBus.changeAge(this.userAge);
	}
}
...
```

