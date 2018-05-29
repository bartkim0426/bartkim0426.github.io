---
layout: post 
title:  "window confirm(): alert 말고 확인 창 띄우기"
categories: js
---


javascript로 `alert` 창만 띄워보아서, `confirm` 창들은 모두 모달 형태로 만들어서 띄워야 하는 줄 알았는데..

`confirm()` 기본 기능이 제공되고 있다는걸 이제서야 알았다.

사용법은 아주 간단하다. `confirm('버튼을 눌러주세요')`와 같이 사용하면 된다.

`confirm()`을 누른 후 나온 창에서 `yes`를 누르면 `true`가, `no`를 누르면 `false`가 전달된다. 이후 로직을 작성하면 된다.


```html
<button onclick="myConfirm()">click to confirm</button>

<script>
function myConfirm() {
    var result = confirm('press ok');
    if (result) {
        alert('do sth here!')
    }
} 
</script>
```

[w3school 여기](https://www.w3schools.com/jsref/met_win_confirm.asp)에서 직접 try 해볼수 있다.
