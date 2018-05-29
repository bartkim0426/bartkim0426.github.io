---
layout: post 
title:  "vue.js - 애니매이션"
categories: vue
---


## `transition`

`<transition></transition>` element를 사용해서 애니매이션을 추가할 수 있다.

버튼을 누르면 아래 div가 나타나는 간단한 코드로 예제를 만들어 보자.
### `App.vue`
```
<temlpate>
  <div>
    <button v-on:click="show = !show">Show info box</button>
    <br>
    <br>
    <transition>
      <div v-if="show">Info box</div>
    </transition>
  </div>
</temlpate>

<script>
export default {
  data() {
    show: true;
  }
}
</script>
```

위의 예제를 보면 `div`를 `<transition>` tag가 감싸고 있는 것을 볼 수 있다. 

> <transition> 태그 안에는 하나의 엘리멘트만 있어야 한다.


## Transition CSS classes
- `*-enter` 클래스
- `*-enter-active` 클래스
- `*-leave`
- `*-leave-active`
> default: v-enter 


## Fade transition
간단한 fade transition을 구현해보자.

transition에 부여한 이름을 `-enter`, `-leave` 클라스에 사용한다.
```javascript
<temlpate>
  <div>
    <button v-on:click="show = !show">Show info box</button>
    <br>
    <br>
    <transition name="fade">
      <div v-if="show">Info box</div>
    </transition>
  </div>
</temlpate>

<script>
export default {
  data() {
    show: true;
  }
}
</script>

<style>
.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  transition: opacity 1s;
}
.fade-leave {
  opacity: 1;
}
.fade-leave-active {
  transition: opacity 1s;
  opacity: 0;
}
</style>
```


## Slide transition

```javascript
<temlpate>
  <div>
    <button v-on:click="show = !show">Show info box</button>
    <transition name="slide">
      <div v-if="show">Info box</div>
    </transition>
  </div>
</temlpate>

<script>
export default {
  data() {
    show: true;
  }
}
</script>

<style>
.slide-enter {
}
.slide-enter-active {
  animation: slide-in 1s ease-out forwards;
}
.slide-leave {
}
.slide-leave-active {
  animation: slide-out 1s ease-out forwards;
}
@keyframes slide-in {
  from {
    transition: translateY(20px);
  }
  to {
    transition: translateY(0)
  }
}

@keyframes slide-out {
  from {
    transition: translateY(0)
  }
  to {
    transition: translateY(20px);
  }
}
</style>
```

## Mixing transition

