---
layout: post 
title:  "Sass 기초 사용법"
categories: sass
---



## Variable
Sass는 variable을 사용해서 자주 쓰는 것들을 쉽게 사용할 수 있다. `$`를 사용해서 var를 선언할 수 있다.
```sass
$color-red: red;

div {
  background-color: $color-red;
}
p {
  background-color: $color-red;
}
```

## Nested
Sass는 중첩된 엘리멘트들을 다 쓰지 않고, 해당 엘리먼트 내에 작성이 가능하게 하여 훨씬 간편하게 스타일을 적용할 수 있다.

```sass
.post {
  color: red;
  .title {
    color: black;
  }
}
```

## Referencing Parent Selectors (부모참조 선택자: `&`)
위의 중첩을 사용할 때, `&`를 사용하면 css로 컴파일 될 때 복합선택이 된다.
```sass
.post {
  color: red;
  .title {
    color: black;
  }
  &.content {
    color: blue;
  }
	body.info& {
    color: white;
	}
}
```
그러면 css로 컴파일 될 때 `.post.content { color: blue }`로 컴파일 된다.

`&`를 뒤에 붙인다면 `.body.info.post { color: white }`로 사용된다.


## Mixin & Selector

반복되는 스타일을 작성해야하는 번거로움을 `mixin`으로 선언하고 `include`로 호출하여 사용이 가능하다.

mixin을 작성할 때 argument를 넣어서 작성도 가능하다. (`$visibility:hidden`처럼 default 값을 줄 수도 있다.)

```sass
@mixin backface-visibility($visibility:hidden) {
  backface-visibility: $visibility;
  -webkit-backface-visibility: $visibility;
  -moz-backface-visibility: $visibility;
  -ms-backface-visibility: $visibility;
  -o-backface-visibility: $visibility;
}
.font {
  @include backface-visibility(hidden);
}
```

## 숫자 연산 가능
`px`이 포함된 숫자 값을 `+, -, *, /, %`로 연산을 할 수 있다.

```sass
$width: 300px;

div {
  font-size: $width/15;
}
```

## `@each`로 looping
```sass
$color-list: (orange, purple, red);

@each $item in $color-list {
  .#{$item} {
	  background: $item;
	}
}
```

## `@for`로 for looping
```sass
$total: 10; //Number of .ray divs in our html
$step: 360deg / $total; //Used to compute the hue based on color-wheel


.ray {
  height: 30px;
}

//Add your for-loop here:
@for $i from 1 through $total {
    ray:nth-child(#{$i}) {
    background: blue;
  }
}
```
## `@include`


## `@extend`

