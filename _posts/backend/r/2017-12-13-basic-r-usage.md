---
layout: post 
title:  "R언어 기본 문법 및 사용법"
categories: r
tags: r
---


R언어는 타 언어들에 비해 데이터 분석에 집중되어 있기 때문에 help system이 상당히 잘 되어 있다. 따로 문법 공부를 많이 하지 않더라도 `help`, `example` 등을 잘 활용하여 설명을 잘 읽으면 쉽게 따라할 수 있다.

> 앞으로 모든 입력 코드는 `>`를 써서 나타냈다. `>` 표시가 없는 코드는 결과값이라고 생각하면 된다.
> `#` 이후에 기록된 것들은 주석이다. 주석은 프로그래밍 상에서 작동되지 않느다.

### 기본 연산
기본 연산은 상식적인 수준에서 사용하면 된다.
```r
> 1 + 1
2
> 2 * 7
14
# 같은지 비교하기 위해서 == 사용
> 2 + 2 == 5
FALSE
```

### TRUE & FALSE
boolean(0과 1로 참 거짓을 나타내는 방식)으로 R은 `TRUE`, `FALSE`를 사용한다.
```
> 1 + 1 == 2
TRUE
# 줄임말로 T, F를 사용할 수 있다
> T == TRUE
TRUE
```

### 변수 선언하기
타 언어들과 다르게, 등호 기호(`=`)를 사용하지 않고 화살표 기호를 사용한다.
> 변수란? 프로그래밍을 처음 접하면 "변수(variable)"라는 것을 보고 혼란이 오기 마련이다. 너무 어렵게 생각할 필요 없이 "이름표"를 붙인다고 생각하면 된다.
> `x <- 5` 코드를 보면, 숫자 5에 x라는 이름표를 붙였다고 보면 된다. 즉, 변수 `x`는 그냥 5를 나타내는 이름표일 뿐 실제로는 5인 것이다.

```
> x <- 24
> y <- "hello"

# 변수가 선언되면 변수로 모든 연산 등이 가능하다.
> x * 2
48
```

### Function: 내부함수들
R은 데이터 분석에 특화된 언어라는 평에 걸맞게 많은 내부 함수들을 지원한다.

**각종 도움함수들**
필요한 함수가 있는데 사용법을 잘 모르겠을 때 사용하면 매우 유용하다.
#### help
`help(함수명)`으로 사용 가능. 도움말을 제공하여 (영어긴 하지만) 사용 방법을 확인 가능하다.
```
> help(sum)
function (..., na.rm = FALSE)  .Primitive("sum")
```
위와 같이 설명이 나오는데, 무슨 말인지 잘 모르겠다면 `example`이 큰 도움이 된다.

#### example
`example(함수명)`으로 사용이 가능. 해당 함수의 예시를 보여준다.
```
> example(min)
min> require(stats); require(graphics)

min>  min(5:1, pi) #-> one number
[1] 1

min> pmin(5:1, pi) #->  5  numbers
[1] 3.141593 3.141593 3.000000 2.000000 1.000000
```
> 모든 함수가 example을 가지고 있는 것은 아니다. example을 가지고 있지 않은 함수에서 example을 사용한다면 (예를들어 `example(sum)`) 다음과 같은 에러 메시지가 나온다.
```
Warning message:
In example­(sum) : ‘s­um’ has a ­help file ­but no exa­mples
You didn't­ call the ­`help` fun­ction! Try­: 'help(re­p)'
```


#### sum
괄호 안에 오는 숫자(및 변수)들의 합
```
> sum(1, 2, 5)
8 > x <- 10
> SUM(5, x)
15
```
#### rep
반복. `rep(반복할것, times=반복횟수)`처럼 사용 가능하다.
```
> repeat("hi", times=3)
"hi" "hi" "hi"
``` 

### Files

#### list
`list.files()`로 해당 디렉토리의 파일들을 확인 가능하다.
```
> list.files()
test1.R, test2.R
```

#### 파일 실행하기
`.R`처럼 R 확장자를 가진 파일을 실행시킬 수 있다.
`source(파일명)`으로 실행 가능하다.
```
> source(test1.R)
```
