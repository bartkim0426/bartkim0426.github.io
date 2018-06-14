---
layout: post 
title:  "vue.js: Multiple search"
categories: vue
---

vue filter with multiple object value.


```javascript
...
computed: {
    filteredData: function() {
      let result = this.data;
      var search = this.search;
      var column_list = ['email', 'name']
      if (search) {
        // with ES5
        // result = result.filter(function (item) {
        //   return column_list.map(function (key) {
        //     return item[key].includes(search);
        //   }).includes(true);
        // });
        // ES6
        result = result.filter(item => column_list.map((key) => item[key].includes(this.search)).includes(true));
      }
      return result;
    },
},
...
```

And use it in html  with `v-for="data in filteredData"`.
