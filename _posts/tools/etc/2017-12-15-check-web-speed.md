---
layout: post 
title:  "웹 사이트 속도 측정하기"
categories: etc
tags: etc speed
---

이번 블랙 프라이데이에 `udmey`에서 엄청난 세일을 해서 강좌를 거의 100불 넘게 구매해두었다. 처음에는 웹/앱 가리지 않고 스트리밍이 제공되어서 참 좋은 서비스라고 생각했는데 어느 순간부터 사이트 로딩이 현저하게 느려지고 동영상도 계속 로딩중인 상태로 멈취있는 일이 잦아졌다. 인터넷 속도를 측정해보니 400MB를 넘는 속도를 내고 있기 때문에 인터넷 문제는 아닐 것이라고 생각하여 브라우저 캐시를 지우고 다른 브라우저들을 사용해보아도 문제는 여전했다.

그래서 과연 나만 그렇게 느린지 해당 사이트의 퍼포먼스(속도)를 측정하기 위해 관련된 사이트들을 찾아보았다.  

### 1. [Pingdom](https://tools.pingdom.com/)
세계적으로 가장 많이 쓰이는 핑 사이트 중 하나. ui가 깔끔하고 URL만 넣어주면 쉽게 사용 가능하다.  (회원가입 불필요)


A부터 E까지 Grade로 직관적으로 결과를 보여주고, Page size, Load time 등도 보여준다. 


아쉬운 점은 test 지역이 미국의 뉴욕, 캘리포니아. 스웨덴과 호주 총 4지역밖에 없는 것.

![pingdom](https://i.imgur.com/mxtszhI.png){:class="img-responsive"}

### 2. [GTmetrix](https://gtmetrix.com)
GT메트릭스는 전 세계의 지역 중 랜덤으로 test server region을 골라서 테스트 해준다. 

> random이라고 하는데 여러번 해도 캐나다 밴쿠버-크롬브라우저로만 매핑이 된다. 설명에는 7개의 다른 지역의 28 서버에서 운영된다고 한다. 아마도 회원가입을 하면 해당 기능을 제공하는듯

A-E까지 등급으로 PageSpeed와 YSlow score를 보여주고, Waterfall도 자세하게 제공해준다. ui가 핑덤만큼 직관적이지는 않지만 깔끔하고 자세한 결과들을 제공한다.

![gtmatrix](/static/assets/img/blog/images/gmatrix.png){:class="img-responsive"}


### 3. [구글 pagespeed insight](https://developers.google.com/speed/pagespeed/insights/)

모바일을 스피드를 보고 싶다면 추천하는 스피드 테스트 사이트. 결과가 모바일/데스크탑으로 나오고 점수에 따라`Good` - `Need work` - `Poor` 3단계로 나오고 가능한 최적화들의 리스트도 보여준다. 사이트를 운영하면서 최적화를 하려면 가장 좋은 도구가 아닐까 싶다.

> 위에서 소개한 두가지의 툴에 비해서 훨씬 정확하게 측정되는 느낌이다 위에서는 Good이 나온 사이트도 pagespeed에서는 35가 나와서 깜짝 놀랐다. 심지어 `google.com`도 모바일 페이지는 `Need work`가 나온다 (열일해라 구글...)

![google pagespeed](https://i.imgur.com/WjEo6p2.png){:class="img-responsive"}

### 4. [WebPageTest](http://www.webpagetest.org/)
만약 특정 브라우저, 지역을 선택적으로 확인해보고 싶으면 이 페이지를 추천한다. 한국, 중국 등을 포함한 백여개의 지역을 선택 가능하고, ios, android등 모바일을 포함한 브라우저도 선택이 가능하다. 심지어 ios10, 11 도 선택이 가능하다. 또한 3번의 테스트를 연속적으로 해서 평균치를 보여준다.

다만 페이지 ui가 직관적이지 않고 결과를 보기가 조금 까다로운 단점이 있다.

![webpagetest](https://i.imgur.com/7flq9k1.png){:class="img-responsive"}

### 5. [LoadImpact](https://loadimpact.com/)
로드입팩트는 위 사이트처럼 일회성 테스트가 아닌 리퀘스트를 계속 날려주면서 특정 시간동안 계속 테스트를 진행한다. 결과를 차트로 보여주며 비회원인 경우에는 기능들이 많이 제한된다.

깔끔한 ui와 다양한 지역의 테스트를 한번에 해주기 때문에 편리하게 사용 가능하지만, 제대로 된 기능을 사용하려면 회원가입을 하고 결제를 해야한다. 그렇지 않으면 한달에 5번만 무료로 사용 가능.

![LoadImpact](https://i.imgur.com/rzDeHXY.png){:class="img-responsive"}

![LoadImpact](https://i.imgur.com/4l7ysCn.png){:class="img-responsive"}
