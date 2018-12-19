---
layout: post 
title:  "vue.js 공식문서: 클래스와 스타일 바인딩 ~ 폼 입력 바인딩"
categories: vue
---

# 클래스와 스타일 바인딩

- `class`, `style`은 `v-bind` 사용. 
- 최종적으로 `string` 형태로 넘기면 되는데 오류 발생 가능성이 높음 => 표현식, 객체 가능

## HTML 클래스 바인딩


### 객체구문
- `v-bind`에 객체 전달 가능 (toggle)

```html
<div v-bind:class="{ active: isActive }"></div>
```

- 여러 객체 사용 가능

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

- 객체는 다음과 같은 형태로 data에 전달해주면 된다.
```javascript
data: {
  isActive: true,
  hasError: false
}
```

- 하지만 보통은 객체 자체를 바인딩해서 사용. inline으로 사용하진 않을듯

```html
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```


### 배열구문

- 배열로도 class를 전달 가능. 여러 클래스 바인딩시에는 이방법 사용할듯?

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

- 여러 클래스 있는 경우에는 배열 안에 객체구문 사용

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```


### 컴포넌트와 함께 사용하는 방법

- 컴포넌트 템플릿 호출시에 property 넘기듯이 class를 넘길 수 있음



## 인라인 바인딩 스타일

### 객체 구문
- css같아보이지만 사실은 javascript. 나중에 css로 렌더링됨 => 그래서 camelCase, kebab-case 사용해야함

```html
<div v-bind:style="styleObject"></div>
```

```javascript
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### 배열 구문
- 위의 class와 마찬가지로 style도 배열로 사용 가능


### 자동 접두사
- 접두사가 필요하면 자동으로 적용

### 다중 값 제공
- 접두사가 있는 여러 값을 배열로 전달
- 2.3.0+ 지원

```html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```



