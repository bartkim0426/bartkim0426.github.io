---
layout: post 
title:  "BTT(Better Touch Tool)로 Safari 동영상 play/pause 단축키"
categories: mac
tags: mac safari
---

요새 udemy 강의를 많이 듣고 있는데, 아무래도 코딩 강의다보니 멈췄다가 다시 실행하는 일이 잦았다. 나는 해피해킹 키보드를 사용중인데, 계속 키보드로 타이핑을 하다가 영상을 멈추고 싶을 때마다 safari로 가서 play/pause를 눌러줘야 하는게 여간 귀찮지 않았다. (아주 사소한 일인데 그놈의 귀차니즘...)

처음에는 맥의 기본 play/pause 버튼이 safari 영상 재생/일시정지에 먹히길래 그걸 `Fn + Enter`에 매핑해서 쓰려고 했는데, window focus가 safari에 맞춰져 있을 때에만 작동하여 매번 safari로 활성화된 window를 바꾼 후 `Fn + Enter`를 눌러야 되서 매우 귀찮았다.

그러던 중, 얼마 전 터치바를 매핑하기 위해 사용하기 시작한 [ `Better Touch Tool` ](https://www.boastr.net)에 매핑하면 어떨까 하는 아이디어가 생각이 나서 매핑해보았다.

BTT에는 다양한 좋은 기능들이 있지만, 무엇보다도 특정 이벤트(키보드, 트랙패드, 터치바, 마우스 등)에 원하는 `AppleScript`를 작성해서 그것을 실행시킬 수 있다는 점이 큰 장점이다. 

>  애플스크립트(AppleScript)는 애플이 만든 스크립트 언어이며, 시스템 7 이후의 매킨토시 운영 체제에 통합되어 있다. 애플스크립트라는 용어는 스크립트 언어 그 자체를 가리키거나, 애플스크립트 언어로 기록한 스크립트를 가리킨다.
> 애플스크립트는 객체 지향 프로그래밍의 요소도 일부 갖고 있는데, 특히 스크립트 객체 구조와 일부 리스프 계열 자연 언어 처리 환경에서 볼 수 있다.
> 출처: [위키백과](https://ko.wikipedia.org/wiki/%EC%95%A0%ED%94%8C%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8)


`udemy`의 video를 재생/일시정지하는 applescript이다. (사실은 자바스크립트를 실행하는 애플스크립트이다.)

```javascript
tell application "Safari"
	activate
	do JavaScript "var vid = document.getElementsByTagName('video')[0];
	var playPause = function() {
		if (vid.parentElement.classList.contains('vjs-paused') == true) {
			vid.play()
		} else {
			vid.pause()
		}
	};
	playPause()
	" in current tab of window 1
end tell
```

위 코드를 다른 웹사이트에 사용하려면 
- 해당 영상이 `video` 태그 (html5)를 사용하여야 하고
- 한 페이지에 영상이 한개만 있어야 하고 (만약 여러개인 경우 해당 영상의 class나 id명을 찾아서 지정해주어야한다)
- 재생중/일시정지 상태임을 확인하는 클라스나 id값을 찾아야한다.
- 실행 후 safari로 window를 활성화 시키지 않고 원래 작업중인 윈도우에 focus를 맞추려면 `activate`를 지우면 된다.
