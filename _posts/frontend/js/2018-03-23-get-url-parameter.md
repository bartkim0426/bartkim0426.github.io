---
layout: post 
title:  "javascript: get Url parameter with javascript"
categories: js
---


There's plenty of solution for get URL parameter with javascript.


## 1. Custom getUrlParam function with JQuery
```javascript
var getUrlParameter = function getUrlParameter(sParam) {
    var sPageURL = decodeURIComponent(window.location.search.substring(1)),
        sURLVariables = sPageURL.split('&'),
        sParameterName,
        i;

    for (i = 0; i < sURLVariables.length; i++) {
        sParameterName = sURLVariables[i].split('=');

        if (sParameterName[0] === sParam) {
            return sParameterName[1] === undefined ? true : sParameterName[1];
        }
    }
};

// usage for example.com/?technology=jquery&blog=jquerybyexample
var tech = getUrlParameter('technology');
var blog = getUrlParameter('blog');
```

## 2. Using `URLSearchParams` (for supported browser)
```javascript
let searchParams = new URLSearchParams(window.location.search)

searchParams.has('send') // true or false
```

## 3. using `jquery.url.js`
If you have to get url params for many time, you can use `jquery.url.js` for simple usage. Also you can get other url information such as `protocol`, `path`, etc.

You have to import [jquery.url.js](https://plugins.jquery.com/url/)

```javascript
<script type="text/javascript" src="jquery.js"></script>
<script type="text/javascript" src="jquery.url.js"></script>

<!-- URL:  www.example.com/correct/?message=done&year=1990 -->

<script type="text/javascript">
$(function(){
    $.url.attr('protocol')  // --> Protocol: "http"
    $.url.attr('path')      // --> host: "www.example.com"
    $.url.attr('query')         // --> path: "/correct/"
    $.url.attr('message')       // --> query: "done"
    $.url.attr('year')      // --> query: "1990"
});
```
