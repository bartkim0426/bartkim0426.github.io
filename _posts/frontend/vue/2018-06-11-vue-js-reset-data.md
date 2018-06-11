---
layout: post 
title:  "vue.js: reset data"
categories: vue
---


```javascript
...
methods: {
    resetData: function() {
      Object.assign(this.$data, this.$options.data());
    },
}
...
```


**`Object.assign()`**

```javascript
Object.assign(target, ...sources)
```
- Properties in the target object will be overwritten by properties in the sources if they have the same key.

> More detail [MDN web docs](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
