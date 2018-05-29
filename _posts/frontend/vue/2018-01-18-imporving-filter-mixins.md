---
layout: post 
title:  "vuejs: Filter, Mixins 심화"
categories: vue
---


# Filters
vue.js는 default filter가 없기 때문에 사용하기 위해서는 직접 만들어야 한다.

## Local filter 만들기
methods를 만드는 것처럼 아주 간단하게 만들 수 있다.

lower case를 모두 upper case로 만들어 주는 필터를 만들어보겠다.
```javascript
<temlpate>
  <p>{{ text | toUpperCase }}</p>
</temlpate>

<script>
export default {
  data() {
    return {
      text: 'Hello There!'
    }
  },
  filters: {
    toUppercase(value) {
      return value.toUpperCase();
    }
  }
}
</script>
```

이제 `HELLO THERE`라고 출력되는 것을 볼 수 있다.

## Global filter 만들기, Filter chain
global filter도 비슷하게 만들 수 있다. `main.js`에 `Vue.filter()`로 등록해 두면 따로 호출할 필요 없이 component에서 사용이 가능하다.

### `main.js`
```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.filter('to-lowercase', function(value) {
  return value.toLowerCase();
})

new Vue({
  el: '#app',
  render: h => h(App)
})
```

이제 `{{ text | toUppercase | to-lowercase }}`와 같이 글로벌 필터를 동시에 적용시킬 수 있다.


## Computed property: filter의 나은 대안
vue의 filter도 괜찮지만, computed property를 사용하는 것이 나을 때가 종종 (많이) 있다. 

`fruits`의 리스트를 만들고, input에 입력한 값과 매칭되는 결과만 뽑는 computed를 작성해보겠다.

```javascript
<temlpate>
  <div>
    <input type="text" v-model="fruitText">
    <ul>
      <li v-for="fruit in filteredFruits">{{ fruit }}</li>
    </ul>
  </div>
</temlpate>

<script>
export default {
  data() {
    fruits: ["apple", "banana", "cherry", "melon"],
    fruitText: ""
  },
  computed: {
  filteredFruits() {
    return this.fruits.filter((element)=>{
      return element.match(this.fruitText);
    });
  }
  }
}
</script>
```

## Computed property의 문제: Duplicate된 코드
이렇게 컴포넌트 내에서 computed를 만들면, 비슷하거나 혹은 같은 방식으로 필터를 하는 모든 컴포넌트들에 해당 코드를 하드코딩해야하는 단점이 있다.

이를 해결해 주는 것이 `Mixins`. `Mixins`을 사용해서 아주 간단하게 중복을 해결할 수 있다.

먼저 관련된 코드를 가지고 있는 `Mixin.js`를 하나 만든다. 이름은 상관 없지만 `fruitMixin.js`로 한다.

```javascript
export const fruitMixin = {
  data() {
    return {
      fruits: ['Apple', 'Banana', "Mango", "Melon"],
      filterText: ''
    }
  },
  computed: {
    filterFruits() {
      return this.fruits.filter((element) => {
        return element.match(this.filterText);
      });
    }
  }
}
```

이제 해당 mixin을 사용할 곳 (여기서는 `App.vue`)에서 믹스인을 불러오기만 하면 된다.

```javascript
<template>
  <div>
    <input type="text" v-model="fruitText">
    <ul>
      <li v-for="fruit in filteredFruits">{{ fruit }}</li>
    </ul>
  </div>
</template>

<script>
import { fruitMixin } from './fruitMixin';

export default {
  mixins: [fruitMixin]
}
</script>
```

이전과 동일한 상태인 것을 볼 수 있다.


## Global mixins

global mixin은 모든 instance, component에 추가되기 때문에 잘 생각해서 사용해야한다. vue.js 공식 문서에서는 웬만하면 local로 필요한 곳에서 사용하라고 추천한다.

실제로 한 페이지에서 component가 2개 사용된다면, global mixin은 3번이나 호출된다. 

### `main.js`에서 `Vue.mixin`으로 생성 가능.
```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.mixin({
  created() {
    console.log('Global Mixin - created hook');
  }
})

new Vue({
  el: '#app',
  render: h => h(App)
})
```

