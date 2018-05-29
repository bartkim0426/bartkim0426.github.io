---
layout: post 
title:  "a tag 비활성화 - disable a tag"
categories: js
---

a 태그를 `disabled` 시키려고 찾아보다가 a tag는 disabled가 없고 `href`를 없애거나 `<a href="javascript:void(0);">`를 사용하면 된다고 해서 `javascript:void(0)`이 뭔지 찾아보았다.

[Mozilla 공식 dev 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/void)에서는 다음과 같이 얘기하고있다.

> `void` 연산자는 주어진 식(expression)을 실행하고 `undefined`를 반환합니다

즉, javascript void 안의 표현식을 실행하고, 이후 `undefined`를 반환하기 때문에 `a` 태그를 무력화시킨다.

다음 두 코드는 똑같이 a 태그를 비활성화 하는 코드이다. `href` 안에 `#`(hash)값 없이 해쉬만 넣으면 아무 일도 일어나지 않기 때문이다.

(`javascript:;`도 `javascript:void(0)`과 동일한 역할을 한다.)

```html
<a href="#"></a>

<a href="javascript:void(0)"></a>
<a href="javascript:;"></a>
```

물론 다음 두 방법 말고도 다양한 방법이있다. 

a에서 `href` 자체를 없애고 따로 css 포인터를 넣어줄 수도 있다. (물론 `a` 태그로서의 가치를 상실하는 것이기 때문에 좋지는 않다.)

```javascript
<a onclick="myFunc();">Hello</a>

$(document).ready(function(){
    $('a').css('cursor', 'pointer');
});
```


그럼 어떤 코드가 더 좋은 코드일까?

모질라 문서에서는 `javascript:` 프로토콜은 이벤트 핸들러의 대안이기 때문에 적극적인 사용을 지양한다고 얘기한다. 뿐만 아니라 a 태그는 기본적으로 앵커이므로, 이동되는 실제 링크가 있는 것이 이상적이다.
(만약 링크가 없는 버튼을 사용하고 싶다면 `button` 태그 등을 사용하는 것이 좋다.)

그래서 사실 위의 모든 방법은 잘못되었다고 할 수 있는 것이다. 링크가 없다면 a 태그 대신 `button`을 사용하자!
