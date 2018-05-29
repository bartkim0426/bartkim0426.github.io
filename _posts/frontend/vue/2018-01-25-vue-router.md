---
layout: post 
title:  "vue.js Router: vue-router"
categories: vue
---

## install vue-router
```bash
npm install --save vue-router
```

## set up vue-router

`main.js`에서 `Vue.use`를 통해 사용할 수 있다.

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router';
import App from './App.vue'

Vue.use(VueRouter);

new Vue({
  el: '#app',
  render: h => h(App)
})
```

## `routes.js` 만들기
`src/routes.js`에 라우트를 선언해준다.
```javascript
import User from './components/user/User.vue';
import Home from './components/Home.vue';

export const routes = [
  { path: '', component: Home },
  { path: '/user', component: User }
];
```

## `main.js`에 라우터 등록하기
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router';
import App from './App.vue'
import { routes } from './routes';

Vue.use(VueRouter);

const router = new VueRouter({
  routes
  // same as (ES6 syntax)
  // routes: routes
})

new Vue({
  el: '#app',
  render: h => h(App)
})
```

## `App.vue`에 해당 라우트된 컴포넌트가 들어갈 위치 지정해주기
```javascript
<template>
  <div>
    <h1>Routing</h1>
    <hr>
    <route-view></route-view>
  </div>
</template>
```

이제 브라우저에서 기본 페이지와 `/user`를 사용할 수 있다.

`vue-router`는 기본으로 해시 모드 (`#`)를 사용하기 때문에 url이 `localhost:8080/#/`로 되어있는 것을 볼 수 있다. 이는 vue-router의 모드로 아래 설명할 것이다.

## Routing mode : Hash vs History

`#` (hash) 앞의 것은 브라우저에 보내고, 뒤의 것은 vue application에 보내서 url 라우팅을 해줌

=> 예쁘지 않기 때문에 기본 url처럼 하려면?

`main.js`에서 mode를 넣어 주어야 한다
```javascript
const router = new VueRouter({
  routes,
  mode: 'history'
})
```
히스토리 모드에 대한 자세한 설명은 [뷰 공식문서의 여기](https://router.vuejs.org/kr/essentials/history-mode.html)를 참고


## Link 만들기 (`router-link`)
vue-router의 링크를 만드려면 `a`태그 대신 `router-link` 태그를 사용하면 된다.

#### `Header.vue`
`a` 태그 대신 `router-link` 태그를 만들고 `to=""`로 vue-router에서 등록한 주소를 입력해 준다.

```javascript
<template>
<ul class="nav nav-pills">
  <li class="nav-item">
    <router-link to="/">Home</router-link>
  </li>
  <li class="nav-item">
    <router-link to="/user">User</router-link>
  </li>
</ul>
</template>
```

#### `App.vue`
위에서 만든 Header를 불러와서 등록해주자.
```javascript
<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Routing</h1>
        <app-header></app-header>
        <hr>
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
  export default {
    components: {
      appHeader: Header
    }
  }
</script>

<style>
</style>
```

## Styling active link
위에서 만든 `Home`, `User` nav를 선택했을 때 `active` 클라스를 추가해보자.

위의 `Header.vue`에서 `<li></li>` 태그를 삭제해주고 다음을 넣어준다.

- 우선 `router-link` 태그에 `tag="li"`를 넣어 해당 컴포넌트의 태그를 리스트로 설정.
- `active-class="active"` 를 넣어서 해당 `li`가 active일 때 `active` 클래스를 넣음
- `Home`에만 `exact`를 넣어서 `Home`이 모든 상황 (`/user` 등)에서 `active` 되지 않게 설정

```javascript
<template>
<ul class="nav nav-pills">
  <router-link to="/" tag="li" active-class="active" exact><a>Home</a></router-link>
  <router-link to="/user" tag="li" active-class="active"><a>User</a></router-link>
</ul>
</template>
```

## URL parameters

url parameter를 넣으려면 `routes.js`에서 url을 선언할 때 `:id` (원하는 파라미터) 를 넣어 주어야 한다.

```javascript
export const routes = [
  { path: '', component: Home },
  { path: '/user/:id', component: User }
];
```

그리고 URL을 사용할 때 `/user/10`과 같이 파라미터를 포함시켜서 사용하면 된다.


## Fetching and using url parameter
`this.$route`를 통해서 위에서 설정한 url parameter를 사용 가능하다.

```javascript
<template>
  <div>
    <h1>The User Page</h1>
    <hr>
    <p>Loaded ID: {{ id }}</p>
    <button v-on:click="navigateToHome">Go to home</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id
    }
  },
  methods: {
    navigateToHome() {
      this.$router.push('/');
    }
  }
}
</script>
```

## URL parameter 바뀌었을 때 반응
url parameter가 바뀌어도 이미 바인딩 된 데이터는 바뀌지 않는다. 이를 해결하기 위해서 `watch()`를 사용

```javascript
<template>
  <div>
    <h1>The User Page</h1>
    <hr>
    <p>Loaded ID: {{ id }}</p>
    <button v-on:click="navigateToHome">Go to home</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id
    }
  },
  watch: {
    '$route'(to, from) {
      this.id = to.params.id;
    }
  },
  methods: {
    navigateToHome() {
      this.$router.push('/');
    }
  }
}
</script>
```

> vue 2.2 버전 이상부터는 params을 `props`로 바로 넘길 수 있게 되었다. 다음과 같이 사용하면 될듯.

`routes.js`에서 `props`로 넘기고 싶은 url에 `props:true`룰 추가해준다. 그럼 해당 컴포넌트에서 `{{ id }}`로 바로 사용이 가능하다.
```javascript
import User from './components/user/User.vue';
import Home from './components/Home.vue';

export const routes = [
    { path: '', component: Home },
    { path: '/user/:id', component: User, props: true }
];
```

자세한 내용은 [vue 라우트 컴포넌트에 속성 전달 (문서)](https://router.vuejs.org/kr/essentials/passing-props.html) 참고


## Sub Route (child route)

`routes.js`에서 `children`으로 sub routing을 할 수 있다.
```javascript
...
export const routes = [
    { path: '', component: Home },
    { path: '/user', component: User, children: [
    { path: '', component: UserStart },
    { path: ':id', component: UserDetail },
    { path: ':id/edit', component: UserEdit },
    ] }
];
```

이제 `router-link`를 사용해서 각각의 sub router로 갈 수 있다.
`UserStart.vue`에서 `UserDetail` 컴포넌트로 가는 `router-link`를 작성.
```javascript
<template>
  <div>
    <p>Please select a User</p>
    <hr>
    <ul class="list-group">
      <router-link 
        tag="li" 
        to="/user/1"
        class="list-group-item" 
        style="cursor: pointer">User 1</router-link>
      <router-link 
        tag="li" 
        to="/user/2"
        class="list-group-item" 
        style="cursor: pointer">User 2</router-link>
      <router-link 
        tag="li" 
        to="/user/3"
        class="list-group-item" 
        style="cursor: pointer">User 3</router-link>
    </ul>
  </div>
</template>

```


## Route의 파라미터 값 참조하기
해당 user 의 detail 페이지로 갔을 때 해당 user의 id (param)을 참조하는 법을 알아보자.

쉽게 `$route.params`로 참조가 가능하다.

```javascript
<template>
  <div>
    <h3>Some User Details</h3>
    <p>User loaded has ID: {{ $route.params.id }}</p>
    <router-link
      tag="button"
      v-bind:to="'/user/' + $route.params.id + '/edit'"
      class="btn btn-primary">Edit user</router-link>
  </div>
</template>
```

## Routes name 활용하기
위에서 `v-bind:to="'/user/' + $route.params.id + '/edit'"` 부분을 `name`을 사용하면 쉽게 쓸 수 있다.

우슨 `routes.js`에서 `UserEdit`에 name을 추가해준다.
```javascript
...
  { path: '/user', component: User, children: [
    { path: '', component: UserStart },
    { path: ':id', component: UserDetail },
    { path: ':id/edit', component: UserEdit, name: 'userEdit' },
  ]}
...
```
그리고 위의 `UserDetail.vue` 파일의 `v-bind`를 변경.

```javascript
<template>
  <div>
    <h3>Some User Details</h3>
    <p>User loaded has ID: {{ $route.params.id }}</p>
    <router-link
      tag="button"
      v-bind:to="{ name: 'userEdit', params: { id: $route.params.id }}"
      class="btn btn-primary">Edit user</router-link>
  </div>
</template>
```


## Using Query parameters
```javascript
<template>
  <div>
    <h3>Some User Details</h3>
    <p>User loaded has ID: {{ $route.params.id }}</p>
    <router-link
      tag="button"
      v-bind:to="{ name: 'userEdit', params: { id: $route.params.id }, query: { locale: 'en', q: 100 }}"
      class="btn btn-primary">Edit user</router-link>
  </div>
</template>
```

이를 조회하려면 `$route.query`로 조회가 가능.
```javascript
<template>
  <div>
    <h3>Edit the User</h3>
    <p>Locale: {{ $route.query.locale }}</p>
    <p>Analytics: {{ $route.query.q }}</p>
  </div>
</template>
```

## Multiple (Named) router view
여러 개의 `router-view`를 사용 가능하다.

`routes.js`에 path에 여러 name을 지정 가능하다.
```javascript
...
export const routes = [
  { path: '', name: 'Home', components: {
    default: Home,
    'header-top': Header
  } },
  { path: '/user', components: {
    default: User,
    'header-bottom': Header
  }, children: [
    { path: '', component: UserStart },
    { path: ':id', component: UserDetail },
    { path: ':id/edit', component: UserEdit, name: 'userEdit' },
  ] }
  // { path: '/user/:id', component: User, props: true }
]
```

`App.vue`에서 `<app-header></app-header>`를 없애고 header-top, header-bottom 이름을 가지고 있는 `router-view`를 추가해보자.
```javascript
<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Routing</h1>
        <!-- <app-header></app-header> -->
        <hr>
        <router-view name="header-top"></router-view>
        <router-view></router-view>
        <router-view name="header-bottom"></router-view>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
  export default {
    components: {
      appHeader: Header
    }
  }
</script>

<style>
</style>
```

## Redirecting
`routes.js`에 redirect로 추가가 가능하다.

```javascript
...
  { path: '/redirect-me', redirect: { name: 'home' } }
  // { path: '/redirect-me', redirect: '/user' }
```

## "Catch All" (Wildcard)
유효하지 않은 url에 대해서 `'/'`으로 리다이렉트 시키려면 `*`(Wildcard)를 사용하면 된다.
```javascript
  { path: '*', redirect: { name: 'home' } }
```

## Animating routing transition
`transition`을 사용해서 쉽게 route간에 트랜지션을 넣을 수 있다.

`App.vue`에 `slide` 트랜지션을 추가한 예시.
```javascript
<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Routing</h1>
        <!-- <app-header></app-header> -->
        <hr>
        <router-view name="header-top"></router-view>
        <transition name="slide" mode="out-in">
          <router-view></router-view>
        </transition>
        <router-view name="header-bottom"></router-view>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'
export default {
  components: {
    appHeader: Header
  }
}
</script>

<style>
    .slide-leave-active {
        transition: opacity 1s ease;
        opacity: 0;
        animation: slide-out 1s ease-out forwards;
    }

    .slide-leave {
        opacity: 1;
        transform: translateX(0);
    }

    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
    }

    @keyframes slide-out {
        0% {
            transform: translateY(0);
        }
        100% {
            transform: translateY(-30px);
        }
    }

    @keyframes slide-in {
        0% {
            transform: translateY(-30px);
        }
        100% {
            transform: translateY(0);
        }
    }
</style>
```

## Passing hash fragment
hash data (`#data`)를 넘겨서 해당 url에서 원하는 구역으로 가고 싶다면 `router-link`에 `v-bind:to`로 넘기는 link 값에 `hash`를 추가해 주면 된다.

```javascript
<template>
  <div>
    <h3>Some User Details</h3>
    <p>User loaded has ID: {{ $route.params.id }}</p>
    <router-link
      tag="button"
      v-bind:to="link"
      class="btn btn-primary">Edit user</router-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      link: { 
        name: 'userEdit',
        params: { 
          id: this.$route.params.id 
        },
        query: {
          locale: 'en', q: 100 
        },
        hash: '#data'
      }
    }
  }
}
</script>
```

## Controlling the scroll behavior
vueRouter를 선언할 때 `scrollBehavior`를 사용하여 스크롤을 컨트롤 할 수 있다.

다음은 `main.js`에서 브라우저의 `savedPosition`이 있다면 그것으로, `hash`값이 있다면 그곳으로 스크롤 다운하게 `scrollBehavior`를 추가한 코드.
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';
import App from './App.vue';
import { routes } from './routes';

Vue.use(VueRouter);

const router = new VueRouter({
  routes,
  mode: 'history',
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition;
    }
    if (to.hash) {
      return { selector: to.hash };
    }
    return {x: 0, y: 0};
  }
})


new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

## Protecting routes with guards

### Using the "BeforeEnter" guard
3가지 방법으로 `BeforeEnter` guard를 사용이 가능하다.
#### 1. `main.js`에서 전역에 `beforeEach` 사용
`main.js`에서 `router.beforeEach`를 사용해서 각각의 라우팅에 guard를 추가할 수 있따.

`router.beforeEach((to, from, next) => {next()})`로 사용하며, `next()`는 반드시 있어야 한다.

`next()`또한 아래 예시에 써 놓은 것처럼 세가지로 사용이 가능.
```javascript
router.beforeEach((to, from, next) => {
  console.log('global beforeEach');
  // Have to set next() to routing
  // 1. Pass nothing - go to next page
  next();
  // 2. Pass false - stop routing
  // next(false);
  // 3. Pass pop or object
  // next({})
});
```
#### 2. `routes.js`에서 `beforeEnter`를 사용하여 원하는 라우터에 등록
`Home`에서면 사용하고 싶다면 `Home` route에 `beforeEnter: (to, from, next) => {next()}`를 추가해준다.

`next()`의 사용법은 위와 동일.

```javascript
...
  { path: '', name: 'Home', components: {
    default: Home,
    'header-top': Header
  } beforeEnter: (to, from, next) => {
    console.log('inside route setup');
    next();
  }},
...
```

#### 3. 해당 component 안에서 `beforeRouteEnter` 사용

```javascript
  beforeRouteEnter(to, from, next) {
    // true/false를 사용하여 해당 페이지 접근을 가능하게 할 수 있다.
    if (true) {
      next();
    } else {
      next(false);
    }
    // beforeRouteEnter는 아직 component가 초기화 되기 전이기 때문에
    // this.link 등 props에 접근이 불가.
    // 꼭 사용하고 싶다면 아래 Callback 방식을 사용.
    // next(vm => {
    //   vm.link;
    // });
  }
```

### Using the beforeLeave guard

