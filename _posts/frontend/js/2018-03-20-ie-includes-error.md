---
layout: post 
title:  "I.E.에서 .includes 함수 오류"
categories: js
---

특정 요소가 `array` 안에 있는지 확인하기 위해 `includes`를 사용했는데, 윈도우에서 다음과 같은 오류가 발생했다.

```
개체가 'includes' 속성이나 메서드를 지원하지 않습니다.
```

찾아보니 `includes`가 엣지 이외에서는 거의 지원되지 않는다고 한다. 

그래서 `indexOf`를 찾아보니 이것도 I.E.에서 약간의 이슈가 있는것 같지만 기본적으로 잘 동작한다.

```javascript
// before code
if (arrayName.includes(sth)) {
    ...
}
// after code
if (arrayName.indexOf(sth) !== -1) {
    ...
}
```

`python`을 사용해서 backend 코딩을 할 때에는 브라우저 이슈를 전혀 생각하지 않았는데, 앞으로는 브라우저간 호환을 생각하고 코딩할 수 있는 습관을 길러야겠다.
