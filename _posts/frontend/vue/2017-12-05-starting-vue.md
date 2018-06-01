---
layout: post 
title:  "Starting vue - vue official document"
categories: vue
tags: vue doc
---


#### Vue 인스턴스


**Vue 인스턴스 만들기**

```javascript
var vm = new Vue({
    // opt
})
```

- vue 앱은 `new Vue`로 만들어진 컴포넌트들로 구성


**속성과 메소드**

- data 객체에 있는 속성을 프록시 처리
- data가 변경되면 화면이 다시 렌더링

> 인스턴스 속성, 메소드 목록이 나와있는 [API reference](https://kr.vuejs.org/v2/api/#search-form) 정리 필요


**인스턴스 라이프사이클 훅**

- vue는 따로 컨트롤러가 없음. 
- vue 인스턴스의 단계에서 라이프사이클 훅이 호출 => 사용자 지정 로직을 사용할 수 있다

**라이프사이클 다이어그램**

![lifecycle diagram](https://kr.vuejs.org/images/lifecycle.png)


#### 템플릿 문법

> 렌더링 함수 직접 작성 가능, `JSX`도 지원.

**보간법(Interpolation)**

- `{{  }}`(이중 중괄호)를 사용한 `Mustache`. 
- 일반 텍스트이기 때문에 html 출력하려면 `v-html` 디렉티브 사용 (`mustache`는 사용 불가하며 `v-bind`를 사용해야한다.
- javascript 표현식도 바인딩 가능.

**directive**

- `v-`로 시작되는 특수 속성. directive 정리는 [블로그](http://seulcode.tistory.com/264)에 따로 해놓음.


#### 계산된 속성과 감시자

위에서 본것같이 템플릿에 자바스크립트 표현식을 바인딩 할 수 있지만, 너무 복잡하고 유지보수가 어렵기 때문에 계산된 속성(`Computed properties`)를 사용하는 것이 좋다.


사용법은 `data`와 같이 `computed`로 바인딩.

```html
<div id="example">
    <p>message: "{{ message }}"</p>
    <p>computed message: "{{ reversedMessage }}"</p>
</div>
```

```javascript
var vm = new Vue({
    el: "#example",
    data: {
        message: "Hello"
    },
    computed: {
        reversedMessage: function() {
            return this.message.split('').reverse().join('')
        }
    }
})
```

**Computed caching vs Methods**

computed property를 사용하는 것 말고도, `method`를 표현식에 사용이 가능하다.

```html
<p>Reversed message by method: {{ reversedMessage() }}</p>
```


```javascript
...
methods: {
    reversedMessage: function() {
        return this.message.split('').reverse().join('')
    }
}
...
```

- 둘 사이의 가장 큰 차이점은 => computed property는 캐싱, method는 렌더링할때마다 메소드 호출.
- 캐싱을 원하면 `computed`, 그렇지 않고 매번 호출하고싶으면 `method`.

**Computed vs Watched**

angular 등에서는 `watch`를 많이 사용하나보다. 이를 사용하여 매번 데이터 변경을 탐지하는 것보다 `computed`를 사용하는 것이 코드의 중복을 줄이고 효과적.

**computed setter**

`computed`는 기본적으로 `getter`만 제공되지만 필요할 경우 `setter`도 사용 가능. 역으로 데이터 업데이트가 가능하다.


**Watchers**

정말 필요한 경우에는 `watch`가 필요할 떄가 있음. 비동기적 상황에서 데이터 변경으로 인한 자원 소모가 많을 때 사용하면 효과적.

- 예시 (yes/no question)

```html
<div id="watch-example">
    <p>
        yes/no question:
        <input v-model="question">
    </p>
    <p>{{ answer }}</p>
</div>
```


```javascript
var watchExampleVM = new Vue({
    el: "#watch-example",
    data: {
        question: "",
        answer: "please make question including question mark"
    },
    watch: {
        question: function (newQuestion, oldQuestion) {
            this.answer = 'waiting...'
            this.getAnswer()
        }
    }
    methods: {
        getAnswer: {
        ...
        }
    }
})
```


