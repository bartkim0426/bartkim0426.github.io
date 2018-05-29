---
layout: post 
title:  "vue.js directives(디렉티브, 지시문) 알아보기"
categories: vue
---

`Direcitve`:  "지시문"이라는 뜻으로 vue 엘리멘트에 사용되는 특별한 속성이다. 지금까지 다뤄온 `v-model`, `v-for` 등이 모두 지시문.

디렉티브는 총 13가지가 있다. vue 2.0의 모든 디렉티브 리스트는 [vue.js 공식 문서의 directives](https://vuejs.org/v2/api/#directives)에서 자세한 확인이 가능하다.

## `v-text`
내부의 값이 Vue 엘리멘트의 변수로 지정된다. `{{ text }}`와 `v-text="text"`는 동일하다고 볼 수 있다.

## `v-html`
vue의 테이터는 태그 자체가 html에 표시된다. vue 엘리먼트의 데이터에 `<b>Hello</b>`를 넣고 html에 뿌려보면 그대로 출력되는 것을 볼 수 있다.

이를 렌더링해서 **Hello**로 출력하고 싶을 때 `v-html` 디렉티브를 사용하면 된다.

## `v-show`
해당 엘리멘트의 `display: none`을 활성화시켜준다. (`true`일 경우)

```html
<temlpate>
	<div v-show="visible"></div>
</temlpate>

<style>
export default {
	data: function() {
		visible: true
	}
}
</style>
```

## `v-if`
조건문. 아래 `v-else`, `v-else-if`와 함께 사용
## `v-else`
## `v-else-if`

## `v-for`
반복문을 사용할 때 사용. `key`라는 스페셜 attribute를 넣어주는 것이 좋음.
```html
<div v-for="item in items" v-bind:key="item.id">
{{ item.text }}
</div>
```
> `v-if`, `v-for`를 같이 사용하면 `v-if`가 우선.

## `v-on`
### 축약: `@`
가장 많이 쓰는 directive 중 하나. 대부분 `@`로 축약해서 사용한다.

여러 modifiers와 함께 사용이 가능하다. (ex. `v-on:click.prevent="doThis"`)

다음은 modifiers list:
- `.stop`: `event.stopPropagation()` 호출
- `.prevent`: `event.preventDefault()` 호출
- `.capture`: 캡쳐모드의 이벤트리스너 추가 (?)
- `.self`: 이 엘리멘트에서 event가 dispatched 됐을 때만 트리거
- `.{keyCode | keyAlias}` : 특정 키에서만 trigger
- `.native`: root element의 native event를 listen (?)
- `.once`: 한번만 트리거! 
- `.left`: 마우스 왼쪽클릭에만 트리거
- `.right`: 마우스 오른쪽클릭에만 트리거
- `.middle`: 마우스 가운데클릭에만 트리거
- `.passive`: attacheds a DOM event with `{passive: true}`(?)

> `.prevent`, `.self`, `.once`, `.{keyCode | keyAlias}` 등을 자주 쓸듯...

다음은 공식홈페이지에 나온 예시
```html
<!-- method handler -->
<button v-on:click="doThis"></button>

<!-- object syntax (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>

<!-- inline statement -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- shorthand -->
<button @click="doThis"></button>

<!-- stop propagation -->
<button @click.stop="doThis"></button>

<!-- prevent default -->
<button @click.prevent="doThis"></button>

<!-- prevent default without expression -->
<form @submit.prevent></form>

<!-- chain modifiers -->
<button @click.stop.prevent="doThis"></button>

<!-- key modifier using keyAlias -->
<input @keyup.enter="onEnter">

<!-- key modifier using keyCode -->
<input @keyup.13="onEnter">

<!-- the click event will be triggered at most once -->
<button v-on:click.once="doThis"></button>
```

## `v-bind`
`v-on`과 더불어 가장 많이 쓰이는 directive. `:`으로 축약해서 많이 사용한다.

3가지 modifiers가 있다. 
- `.prop`: attribute 대신에 DOM property를 바인딩 해준다. ([뭐가 다른지 설명한 글은 여기](https://stackoverflow.com/questions/6003819/what-is-the-difference-between-properties-and-attributes-in-html#answer-6004028))
- `.camel`: kebab-case 속성 (예를들면 `app-blog`)을 camelCase로 변경 (`appBlog`)
- `.sync`: a syntax sugar that expands into a v-on handler for updating the bound value.a syntax sugar that expands into a v-on handler for updating the bound value. (?)

다음은 사용 예시
```html
<!-- bind an attribute -->
<img v-bind:src="imageSrc">

<!-- shorthand -->
<img :src="imageSrc">

<!-- with inline string concatenation -->
<img :src="'/path/to/images/' + fileName">

<!-- class binding -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">

<!-- style binding -->
<div :style="{ fontSize: size + 'px' }"></div>
<div :style="[styleObjectA, styleObjectB]"></div>

<!-- binding an object of attributes -->
<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

<!-- DOM attribute binding with prop modifier -->
<div v-bind:text-content.prop="text"></div>

<!-- prop binding. "prop" must be declared in my-component. -->
<my-component :prop="someThing"></my-component>

<!-- pass down parent props in common with a child component -->
<child-component v-bind="$props"></child-component>

<!-- XLink -->
<svg><a :xlink:special="foo"></a></svg>
```

## `v-model`
데이터 바인딩시 사용. `<input>`, `<select>`, `<textarea>`에서만 사용 가능하다.

modifiers는 3가지
- `.lazy`: input 대신에 change event를 listen => 입력할때마다 값이 바인딩되지 않고 나중에 변경
- `.number`: string to number
- `.trim`: trim input

## `v-pre`
엘리멘트의 compile을 건너뜀. 그대로 출력 가능하다.

## `v-cloak`
`[v-cloak]{ display: none }`과 함께 사용하며, js를 모두 불러올 때까지 다른 un-compiled된 html을 숨겨준다. 

다음은 예시
```css
[v-cload] {
	display: none;
}
```
{% raw %}
```html
<div v-cloak>
Message: {{ message }}
</div>
```
{% endraw %}


## `v-once`
엘리멘트, 컴포넌트를 한번만 렌더한다. update perfomance를 최적화할때 사용.
