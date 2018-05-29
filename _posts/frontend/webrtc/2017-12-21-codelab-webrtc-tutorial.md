---
layout: post
title:  "WebRTC codelab tutorial"
categories: frontend
tags: frontend api webrtc
---

[Codelab의 WebRTC tutorial](https://codelabs.developers.google.com/codelabs/webrtc-web/#0)을 통한 WebRTC 정리


## 1. Introduction

> WebRTC는 audio, video, data를 웹과 native 앱 등에서 realtime으로 커뮤니케이션 할 수 있게 해주는 오픈 소스 프로젝트

WebRTC는 몇몇 javascript API를 가진다
- `getUserMedia()`: capture audio, video
- `MediaRecorder`: record audio, video
- `RTCPeerConnection`: user들 간에 audio, video 스트리밍
- `RTCDataCh`: user들 간에 data 스트리밍

### 어디서 WebRTC를 쓸 수 있는지?
Firefox, opera, chrome (desktop, android)와 IOS, Android에서 사용 가능

### Signaling이란 무엇인가? 
WebRTC는 RTCPeerConnection을 사용하여 브라우저간의 data streaming을 communicate. `signaling`이라고 불리는 메카니즘이 필요함.

이번 codelab에서는 `Socket.IO` 사용. 하지만 다양한 [대안](https://github.com/muaz-khan/WebRTC-Experiment/blob/master/Signaling.md)들도 존재


### STUN, TURN이란?
WebRTC는 peer-to-peer로 작동하게 디자인되어있기 때문에 사용자들은 direct route로 연결이 가능하다. 하지만 WebRTC는 실제 networking에 대처할 수 있게 만들어져있음: client application은 [ `NAT gateways` ](https://en.wikipedia.org/wiki/NAT_traversal), 방화벽 등을 넘어야하고, 직접 접속(direct connection)이 끊어질 경우의 fallbacks도 필요하다. 		

이런 목적으로 WebRTC API는 STUN 써버를 사용하여 해당 computer의 ip 주소를 얻고, TURN 서버를 peer-to-peer 커뮤니케이션이 실패했을 경우의 relay server로 사용

### WebRTC의 보안?
Encryption은 모든 WebRTC 컴포넌트에 필수적. JavaScript APIs는 secure origin(HTTPS, localhost)에서만 사용 가능.


## 2. Overview

### What you'll learn
- webcam을 통해 video 가져오기
- `RTCPeerConnection`을 통해 video stream
- `RTCDataChannel`을 통해 data stream
- 메세지 교환을 위한 signaling service 세팅
- peer connection과 signaling combine 
- 사진을 찍고 data channel을 통해 이를 공유

### What you'll need
- chrome (>= 47 version)
- Web server for chrome (or use own web server)
- sample code
- 텍스트 에디터
- HTML, CSS, Javascript에 대한 기본 지식


## 3. 샘플 코드 받기

### 코드 다운받기
git을 활용해서 원하는 dir에 받아준다.
```
git clone https://github.com/googlecodelabs/webrtc-web
# 만약 현재 dir에 받으려면
# git clone https://github.com/googlecodelabs/webrtc-web .
```

아니면 [이 페이지](https://codelabs.developers.google.com/codelabs/webrtc-web/#2)에서 소스코드를 다운받을 수 있다.

### Install and verify web server
자신의 웹 서버를 사용해도 되지만, codelab 강의에서는 Chrome web server를 사용한다.

[여기](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en)에서 크롬 익스텐션으로 설치해준다.

chrome 북마크바의 app(앱)을 클릭해서 익스텐션 실행

실행한 뒤 `CHOOSE FOLDER`를 누른 후 아까 다운받은 소스 코드가 있는 디렉토리를 지정해 준다. 나머지 세팅은 그대로 둔 후 (`Automatically show index.html`에만 체크)

`localhost:8887`에 접속했을 때 다음 화면이 뜨면 잘 구동 된 것이다.
![chrome-web-server](https://i.imgur.com/st2NH4X.png){:class="img-responsive"}


## 4. webcam으로 Video 스트리밍하기

### What you'll learn
- webcam으로 video stream
- stream 재생 다루기
- CSS, SVG를 사용하여 video 다루기

> 완성된 버전은 소스코드의 `step-01` 폴더에서 찾을 수 있음

### A dash of HTML...
아까 지정한 work 디렉토리의 `index.html`에 `video`, `script` 엘리먼트 추가
```html
<body>
	<h1>Realtime communication with WebRTC</h1>
	<video autoplay></video>
	<script src="js/main.js"></script>
</body>
```

### ...and a pinch of JavaScript
work 디렉토리 밑의 `js` 폴더에 `main.js` 생성
```javascript
'use strinct';

// 이부분은 무슨의미?
// prefixed name이라고 함
navigator.getUserMedia = navigator.getUserMedia ||
	navigator.webkitGetUserMedia || navigator.mozGetUserMedia;

var constraints = {
	audio: false,
	video: true
};

var video = document.querySelector('video');

function successCallback(stream) {
	window.stream = stream; // console에서 stream이 가능하게
	if (window.URL) {
		video.src = window.URL.createObjectURL(stream);
	} else {
		video.src = stream;
	}
}

function errorCallback(error) {
	console.log('navigator.getUserMedia error: ', error);
}

// navigator.getUserMedia 사용법
navigator.getUserMedia(constraints, successCallback, errorCallback);
```

> 맨 윗줄의 `use strict;`는 common coding gotchas를 피하기 위한 것이다. 		
> 자세한 내용은 [ECMAScript 5 Strict Mode, JSON, and More.](https://johnresig.com/blog/ecmascript-5-strict-mode-json-and-more/)에서 확인 가능

### Try it out
이제 `localhost:8887`를 띄워보면 (카메라를 혀용하시겠습니까라는 게 뜨면 허용을 해줌) 정상적으로 웹캠이 동작하는 것을 볼 수 있다.


### How it works
`getUserMedia()`는 다음과 같이 호출 가능
```javascript
navigator.getUserMedia(constraints, successCallback, errorCallback);
```

이 기술은 아직 최신 기술이기 때문에 브라우저의 맨 앞에 `getUserMedia`라는 prefixed name을 필요로 한다. 
```javascript
// 이부분  
navigator.getUserMedia = navigator.getUserMedia ||
	navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
```

`constraints` arg는 어떤 미디어를 허용할 것인지 - video or(and) audio
```javascript
var constraints = {
	audio: false,
	video: true
};
```
`getUserMedia`가 성공하면, video stream이 video element의 src에 추가된다.


### Bonus points
- `getUserMedia()` 에 전달된 `stream` 객체는 global scope이기 때문에 console에서 불러와 사용 가능하다.
- `stream.getVideoTracks()`: 현재 재생되는 video tracks의 리스트
- `stream.getVideoTracks()[0].stop()`: 비디오 중지
- `constrants` object에 `{audio: true, video: true}`로 하면? => `stream.getAudioTracks`으로 오디오 객체 리스트들에 접근 가능하다. `.stop()`도 사용 가능
- 사이즈 조절은?  constraints 객체에서 수정 가능
```javascript
var constraints = {
	audio: true,
	video: {
		"width": {
			"min": "300",
			"max": "640",
		},
		"height": {
			"min": "200",
			"max": "480",
		}
	}
};
```
- video element에 CSS filter를 추가해보자 
```css
video {
	-webkit-filter: blur(4px) invert(1) opacity(0.5);
}
```
- video element에 SVG filter를 추가해보자

```css
video {
   filter: hue-rotate(180deg) saturate(200%);
   -moz-filter: hue-rotate(180deg) saturate(200%);
   -webkit-filter: hue-rotate(180deg) saturate(200%);
 }
```

##  5. `RTCPeerConnection`을 사용하여 video stream

### What you'll learn
- WebRTC shim인 `adapter.js`를 사용해서 abstract away browser difference(?? 원문 - Abstract away browser differences with the WebRTC shim, adapter.js.)
- RTCPeerConnection API를 사용하여 video stream
- media capture, streaming을 컨트롤

### What is `RTCPeerConnection`?
`RTCPeerConnection`은 WebRTC가 비디오, 오디오를 스트리밍하거나 정보를 교환할 때 호출하는 api.

아래 예시는 두 `RTCPeerConnection` object간의 연결 세팅

### Add video elements and control buttons
`index.html`의 video element를 두개의 video element와 세개의 버튼으로 대체
```html
<video id="localVideo" autoplay></video>
<video id="remoteVideo" autoplay></video>

<div>
	<button id="startButton">Start</button>
	<button id="callButton">Call</button>
	<button id="hangupButton">Hang Up</button>
</div>
```
한개의 video element는 `getUserMedia()`로부터 스트리밍을 하고, 나머지 한개는 `RTCPeerConnection`을 통한 스트리밍. (실제application에서는 한개의 video는 local, 나머지 한개는 remote)

### Add the adapter.js shim
`main.js` 링크 위에 `adapter.js`(예시 소스코드에 있음) link를 추가
```html
<script src="js/lib/adapter.js"></script>
<script src="js/main.js"></script>
```

> [ `adapter.js` ](https://github.com/webrtc/adapter)란? 		
> adapter.js is a shim to insulate apps from spec changes and prefix differences.
> In fact, the standards and protocols used for WebRTC implementations are highly stable, and there are only a few prefixed names.

> In this step, we've linked to a local copy of the most recent version of adapter.js — fine for a codelab, but not good practice for a production app. The adapter.js GitHub repo explains techniques for making sure your app always accesses the most recent version.

> For full WebRTC interop information, see webrtc.org/web-apis/interop.

`index.html`은 다음과 같이 완성된다.
```html
<!DOCTYPE html>
<html>

<head>

  <title>Realtime communication with WebRTC</title>

  <link rel="stylesheet" href="css/main.css" />

</head>

<body>

  <h1>Realtime communication with WebRTC</h1>

  <video id="localVideo" autoplay></video>
  <video id="remoteVideo" autoplay></video>

  <div>
    <button id="startButton">Start</button>
    <button id="callButton">Call</button>
    <button id="hangupButton">Hang Up</button>
  </div>

  <script src="js/lib/adapter.js"></script>
  <script src="js/main.js"></script>

</body>

</html>
```


### Install the RTCPeerConnection code
`main.js`를 `step-02` 폴더 안에 있는 것으로 대체


### Make the call
`index.html`을 열고, `Start` 버튼을 눌러서 웹캠으로부터 비디오를 가져온 후 `Call` 버튼을 눌러 peer connection을 시작. console을 보면 WebRTC logging이 됨


### How it works

> 이 부분을 건너뛰고 다음 설명을 진행해도 무방

WebRTC는 `RTCPeerConnection API`를 사용하여 `peer`로 불리는 WebRTC clients간의 연결을 세팅

위 예시에서는 두 `RTCPeerConnection` object는 같은 페이지에 있다. (실용적이지는 앟지만 API가 어떻게 동작하는지 잘 설명해준다.)

WebRTC peers들 간의    call setting은 세 단계로 구성
- 각각의 호출의 끝에 `RTCPeerConnection`을 생성하고 `getUserMedia()`로부터 local stream을 추가
- network 정보를 get한 후 share: 잠재적인 connection endpoint는 [ `ICE` ](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment) candidates로 알려짐
- local & remote description을 get & share: [ `SDP` ](https://en.wikipedia.org/wiki/Session_Description_Protocol) 포멧에 맞는 local media의 Metadata


Alice와 Bob이 video chat을 하기 위해 RTCPeerConnection을 사용한다고 가정. 		
먼저 Alice와 Bob은 network information을 교환한다. `finding candidates`라는 표현은 `ICE` framework를 사용하여 네트워크 인터페이스와 포트를 찾는 과정을 얘기한다.

1. Alice가 `onicecandidate` 핸들러와 함께 `RTCPeerConnectin` object 생성. `main.js`의 다음 부분과 같음
```javascript
pc1 = new RTCPeerConnection(servers);
trace('Created local peer connection object pc1');
pc1.onicecandidate = function(e) {
  onIceCandidate(pc1, e);
};
```

2. Alice가 `getUserMedia()`를 호출하고 stream을 add
```javascript
pc1.addStream(localStream);
```

3. network candidates가 가능해질 때 step1의 `onicecandidate` 핸들러가 호출됨

4. Alice가 serialized 된 candidate data를 Bob에게 보냄 : 실제 application에서는 `signaling`이라고 불리는 이 프로세스가 messaging server에서 일어남

5. Bob이 candidate message를 받으면, `addIceCandidate()`를 호출하여 remote peer description에 candidate를 추가

```javascript
function onIceCandidate(pc, event) {
	if (event.candidate) {
		getOtherPc(pc).addIceCandidate(
			new RTCIceCandidate(event.candidate)
		).then(
			function() {
				onAddIceCandidateSuccess(pc);
			},
			function(err){
				onAddIceCandidateError(pc, err);
			}
		);
		trace(getName(pc)) + ' ICE cantitate: \n' + event.candidate.cantidate);
	}
}
```

> 이 부분은 자세히 이해하지 못했다. 추후 다시 [codelab 설명](https://codelabs.developers.google.com/codelabs/webrtc-web/#4)을 읽은 뒤 정리 할 예정


### Bonus points
1. `chrome://webrtc-internals/`을 살펴보라 - WebRTC stats과 debugging data 제공 (크롬의 URLs의 full list는 `chrome://about`)
2. CSS를 활용해서 페이지 꾸미기
- video를 side로 보내기
- button을 같은 width, 큰 글자로 만들기
- layout이 모바일에서 작동하게 하기

3. 크롬 개발자 도구에서 `localStream`, `pc1`, `pc2` 살펴보기
4. 콘솔에서 `pc1.localDescription` 보기 - SDP 포멧이란?

### Tips
- There's a lot to learn in this step! To find other resources that explain RTCPeerConnection in more detail, take a look at webrtc.org/start. This page includes suggestions for JavaScript frameworks — if you'd like to use WebRTC, but don't want to wrangle the APIs.
- Find out more about the adapter.js shim from the adapter.js GitHub repo.
- Want to see what the world's best video chat app looks like? Take a look at AppRTC, the WebRTC project's canonical app for WebRTC calls: app, code. Call setup time is less than 500 ms.



## 6. Use RTCDataChannel to exchange data

### What you'll learn
 
- WebRTC endpoints (peers) 간에 data 교환

### Update your HTML
먼저 WebRTC data 채널을 사용해서 두 `textarea` 필드간에 text를 교환.

video, button을 지우고 textarea, button을 추가
```html
<textarea id="dataChannelSend" disabled
placeholder="Press Start, enter some text, then press Send."></textarea>
<textarea id="dataChannelReceive" disabled></textarea>

<div id="buttons">
<button id="startButton">Start</button>
<button id="sendButton">Send</button>
<button id="closeButton">Stop</button>
</div>
```

`main.js`를 `step-03/js/main.js`로 교체 (혹은 `step-03/js/main.js` 호출)


이제 `Start` 버튼을 누르고 내용을 작성한 후 `Send`를 누르면 다른 `textarea`에서 적은 내용이 나타나는 것을 볼 수 있다.


### How it works
text 메세지를 교환하기 위해서 `RTCPeerConnection`과 `RTCDataChannel`을 사용한다.

`sendData()`와 `createConnection()` 기능이 다음 코드에 있음
```javascript
function createConnection() {
	dataChannelSend.placeholder = '';
	var servers = null;
	pcConstraint = null;
	dataConstraint = null;
	trace('Using SCTP based data channels')
	// For SCTP, reliable and ordered delivery is true by default.
	// Add localConnection to global scope to make it visible
	// from the browser console.
	window.localConnection = localConnection = 
		new RTCPeerConnection(servers, psConstraint);
	trace('Created local peer connection object localConnection');

	sendChannel = localConnection.createDataChannel('sendDataChannel', dataConstraint);
	trace('Created send data channel');

	localConnection.onicecandidate = iceCllback1;
	sendChannel.onopen = onSendChannelStateChange;
	sendChannel.onclose = onSendChannelStateChange;

  // Add remoteConnection to global scope to make it visible
  // from the browser console.
	window.remoteConnection = remoteConnection = newRTCPeerConnection(servers, pcConstraint);
	trace('Created remote peer connection object remoteConnection')

	remoteConnection.onicecandidate = iceCallback2;
	remoteConneciton.ondatachannel = receiveChannelCallback;

	lacalConnection.createOffer().then(
		gotDescription1,
		onCreateSessionDescriptionError
	);
	startButton.disabled = true;
	closeButton.disabled = false;
}

function sendData() {
	var data = dataChannelSend.value;
	sendChannel.send(data);
	trace('Send Data: ' + data);
}
```

`dataConstraint`를 사용하는 것을 주목 -  Data channels can be configured to enable different types of data sharing — for example, prioritizing reliable delivery over performance. You can find out more information about options at Mozilla Developer Network.

> 3가지 constraints type
> WebRTC 다른 타입이 모두 `constraints`로 불려져서 혼란스러울 수 있음
> 여기서 constraint와 옵션들을 볼 수 있따.
> - [`RTCPeerConnection`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/RTCPeerConnection)
> - [`RTCDataChannel`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection/createDataChannel)
> - [`getUserMedia()`](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)

### Find out more
- [ WebRTC data channels ](https://www.html5rocks.com/en/tutorials/webrtc/datachannels/) (a couple of years old, but still worth reading)
- [ Why was SCTP Selected for WebRTC's Data Channel? ](https://bloggeek.me/sctp-data-channel/)


## 7. Set up a signaling service to exchange messages


### What you'll learn
- `npm`을 사용해서 `package.json`에 특정된 프로젝트 dependencies를 설치
- `Node.js` 서버를 띄우고 node-static을 사용하여 statice file을 제공
- `Socket.IO`를 사용해서 Node.js에 메세지 서비스를 세팅
- 메시지를 교환하기 위한 `방` 생성

### Concepts
WebRTC 호출을 세팅하고 유지하기 위해서 WebRTC client(peer)들은 metadata를 교환해야한다: 
- Candidate(network) 정보
- resolution, codecs 등과 같은 media 정보를 제공하는 메세지를 요청하고 응답

다시 말해 audio, video, data를 peer-to-peer streaming 하기 전에 metadata를 교환해야 한다는 것인데, 이를 `Signaling`이라고 한다.

이전 단계에서, RTCPeerConnection의 sender & receiver는 같은 페이지에 있었기 때문에 `signaling`은 object들 간의 metadata 전달로 간단. 

하지만 실제 어플리케이션에서는, RTCPeerConnection의 sender & receiver가 다른 기기의 web page에 있기 때문에 metadata를 전달할 방법을 찾아야한다 		
=> 이를 위해 `signaling server` 사용: 서버는 WebRTC clients(peer) 간의 메세지 전송을 할 수 있게 해준다. 실제 메세지는 plain text: `stringified JavaScript object`


### Prerequisite: Install Node.js

In order to run the next steps of this codelab (folders step-04 to step-06) you will need to run a server on localhost using Node.js.

You can download and install Node.js from this link or via your preferred package manager.

Once installed, you will be able to import the dependencies required for the next steps (running npm install), as well as running a small localhost server to execute the codelab (running node index.js). These commands will be indicated later, when they are required.


### About the app
WebRTC는 client-side JavaScript API를 사용하지만, 실제 사용할때는 STUN, TRUN 서버와 함께 signaling(messaging) 서버도 필요하다. [여기](https://www.html5rocks.com/en/tutorials/webrtc/infrastructure/) 더 자세한 설명 	 

이번 step에서는 간단한 Node.js signaling 서버를 만들고, Socket.IO를 사용. 

> 알맞은 signaling server 고르기
> codelab 예시에서는 `Socket.IO`를 signaling server에 사용
> Socket.IO의 디자인은 메세지를 교환하는데 알맞게 되어있고, Socket.IO는 'rooms'라는 빌트인 컨셉 덕분에 WebRTC의 signaling에 걸맞음
> production service에서는, 다른 대안들도 있음. [How to Select a Signaling Protocol for Your Next WebRTC Project.](https://bloggeek.me/siganling-protocol-webrtc/) 참고 => Socket.IO, SocketJS 등.. 어쨌든 Node를 사용해야하는듯

이번 예시에서, server는 `index.js`에 나와있고 clinent가 웹앱에서 이를 구동 수 있다.(`index.html`)  		

`Node.js` application은 두가지 tasks를 가짐

1. it acts as a message relay
```javascript
socket.on('message', function (message) {
	log('Got message', message);
	socket.broadcast.emit('message', message);
})
```

2. it manages WebRTC video chat 'rooms':
```javascript
if (numClients === 1) {
	socket.join(room);
	socket.emit('created', room, socket.id);
} else if (numClients === 2) {
	socket.join(room);
	socket.emit('joined', room, socket.id);
	io.sockets.in(room).emit('ready');
} else { // max two client
	socket.emit('full', room);
}
```

### HTML & JavaScript
`index.html`은 다음과 같다
```
<!DOCTYPE html>
<head>
	<title>WebRTC</title>
	<link rel="stylesheet" href="css/main.css">
</head>
<body>
	<h1>WebRTC</h1>
	<script src="/socket.io/socket.io.js"></script>
	<scrtipt src="js/main.js"></scrtipt>
</body>
</html>
```

`js/main.js`는 다음과 같다.
```javascript
'use strict';

var isInitiator;

window.room = prompt('Enter room name:');

var socket = io.connect();

if (room !== "") {
	console.log("Message from client: Asking to join room " + room);
	socket.emit("create or join", room);
}

socket.on('created', function(room, clientId) {
	isInitiator = ture;
});

socket.on('full', function(room) {
	console.log("Message from the client: Room " + room + " is full");
});

socket.on('ipaddr', function(ipaddr) {
	console.log("Message from the client: Server IP address is " + ipaddr);
});

socket.on('joined', function(room, clientId) {
	isInitiator = false;
});

socket.on('log', function(array) {
	console.log.apply(console, array)
});
```

### Set up socket.IO to run on Node.js
html 파일에서, Socket.IO를 사용하는 부분을 봤을것이다.
```html
<script src="/socket.io/socket.io.js"></script>
```

work directory에 `package.json`을 만들고 다음 내용을 넣어준다.
```json
{
	"name": "webrtc-codelab",
	"version": "0.0.1",
	"description": "WebRTC codelab",
	"dependencies": {
		"node-static": "^0.7.10",
		"socket.io": "^1.2.0"
	}
}
```

이것은 Node Package Manager(`npm`)에게 설치할 project dependencies를 알려주는 app menifest. 	

dependencies를 설치하기 위해서는 command line에 다음을 적어준다. (work directory에서)
```bash
npm install
```

> `npm WARN webrtc@0.0.1 No license field.` 에러가 발생하면 `"license": "UNLICENSED"`를 `package.json`에 추가


`package.json`에 있는 내용들이 설치가 되었으면 , work directory에 `index.js`를 만들고 다음 내용을 넣어준다.
```javascript
'use strict';

var os = require('os');
var nodeStatic = require('node-static');
var http = require('http');
var socketIO = require('socket.io');

var fileServer = new(nodeStatic.Server)();
var app = http.createServer(function(req, res) {
	fileServer.serve(req, res);
}).listen(8080);

var io = socketIO.listen(app);
io.sockets.on('connection', function(socket) {
	function log() {
		var array = ["Message from server: "];
		array.push.apply(array, argument);
		socket.emit('log', array);
	}

	socket.on('message', function(message) {
		log('Client said: ', message);
		socket.broadcast.emit('message', message);
	});

	socket.on('create or join', function(room) {
		log('Received request to create or join room ' + room);

		var numClients = io.sockets.sockets.length;
		log('Room ' + room + ' now as ' + numClients + 'client(s)');

		if (numClients === 1) {
			socket.join(room);
      log('Client ID ' + socket.id + ' created room ' + room);
			socket.emit('created', room, socket.id);
		} else if (numClients === 2) {
      log('Client ID ' + socket.id + ' joined room ' + room);
			io.sockets.in(room).emit('join', room);
			socket.join(room);
			socket.emit('joined', room, socket.id);
			io.sockets.in(room).emit('ready');
		} else {
			socket.emit('full', room);
		}
	});

	socket.on('ipaddr', function() {
		var ifaces = os.networkInterfaces();
		for (var dev in ifaces) {
			ifaces[dev].forEach(function(details) {
				if (details.family === 'IPv4' && details.address !== '127.0.0.1') {
					socket.emit('ipaddr', details.address);
				}
			});
		}
	});
})
```

이제 `localhost:8080`에 접속해서 방 번호를 입력하고, 다른 탭에서 같은 번호를 입력하면 console에 로그가 찍히는 것을 볼 수 있따.


## 8. Combine peer connection and signaling 

### What you'll learn
- Node.js 위에 Socket.IO를 통해서 WebRTC signaling service를 running
- 그걸 이용해서 peers 간의 WebRTC metadata 교환

### HTML & JavaScript
- `index.html` 		

```html
<body>
	<h1>Realtime communication with WebRTC</h1>

	<div id="videos">
		<video id="localVideo" autoplay muted></video>
		<video id="remoteVideo" autoplay></video>
	</div>

	<script src="/socket.io/socket.io.js"></script>
	<script src="js/lib/adapter.js"></script>
	<script src="js/main.js"></script>
```

- `main.js`를 `step-05/js/main.js`로 대체

### Run the Node.js server

```bash
node index.js
```
`localhost:8080`을 열고, 새로운 탭이나 창에 다시 `localhost:8080`을 열면 console.log에서 확인이 가능하다.


## 9. Take a photo and share it via a data channel

### What you'll learn
- 사진을 찍고 canvas element를 사용해서 data 얻기
- remote user와 image data 교환

### How it works
이전에 RTCDataChannel을 사용해서 text message를 교환했었다. 이번에는 전체 파일을 교환 - `getUserMedia()`를 사용하여 캡쳐된 사진 		

핵심프로세스는 두가지 		
1. data 채널 확립.  		 
2. `getUserMdeia()`를 사용해서 webcam video stream을 캡쳐  		 
```javascript
var video = document.getElementById('video');

function grabWebCamVideo() {
	console.log("Getting user media (video) ...");
	navigator.mediaDevices.getUserMedia({
		audio: false,
		video: ture
	})
	.then(gotStream)
	.catch(function(e) {
		alert('getUserMedia() error: ' + e.name);
	});
}
```

3. `Snap`버튼을 누르면 `canvas` element에서 video stream에서 스냅샷을 get
```javascript
var photo = document.getElementById('photo');
var photoContext = photo.getContext('2d');

function sanPhoto() {
	photoContext.drawImage(video, 0, 0, photo.width, photo.heigth);
	show(photo, sendBtn);
}
```

4. `Send` 버튼을 누르면 image를 bytes로 변환하여 data channel을 통해 보낸다.
```javascript
function sendPhoto() {
	// Split data channel message in chunks of this byte length
	var CHUNK_LEN = 64000;
	var img = photoContext.getImageData(0, 0, photoContextW, photoContextH),
		len = img.data.byteLength,
		n = len / CHUNK_LEN | 0;
	console.log('Sending a total of ' + len + ' byte(s)');
	dataChannel.send(len);

	// split the photo and send in chunks of about 64kb
	for (var i = 0; i < n; i++) {
		var start = i * CHUNK_LEN,
			end = (i + 1) * CHUNK_LEN;
		console.log(start + ' - ' + (end -1));
		dataChannel.send(img.data.subarray(start, end));
	}

	// send the reminder, if any
	if (len % CHUNK_LEN) {
		console.log('last' + len % CHUNK_LEN + ' byte(s)');
		dataChannel.send(img.data.subarray(n * CHUNK_LEN));
	}
}
```

5. receiving side에서 data channel에서 받은 message를 bytes에서 image로 변환 후 user에게 보여주기
```javascript
function receiveDataChromeFactory() {
	var buf, count;

	return function onmessage(event) {
		if (typeof event.data === 'string') {
			buf = window.buf = new Uint8ClampedArray(parseInt(event.data));
			count = 0;
			console.log('Expecting a total of ' + buf.byteLength + ' bytes');
			return;
		}

		var data = new Uint8ClampedArray(event.data);
		buf.set(data, count);

		count += data.byteLength;
		console.log('count ' + count);

		if (count === buf.byteLength) {
			console.log('Done. Rendering photo.');
			renderPhoto(buf);
		}
	};
}

function renderPhoto(data) {
	var canvas = document.createElement('canvas');
	canvas.width = photoContextW;
	canvas.height = photoContextH;
	canvas.classList.add('incomingPhoto');
	trail.insertBefore(canvas, trail.firstChild);

	var context = canvas.getContext('2d');
	var img = context.createImageData(photoContextW, photoContextH);
	img.data.set(data);
	context.putImageData(img, 0, 0);
}
```


### Get the code
`step-06`의 파일들로 work directory의 파일들 대체 		

`index.html`
```html
<body>
	<h1>Realtime communication with WebRTC</h1>

	<div id="videoCanvas">
		<video id="camera" autoplay></video>
		<canvas id="photo"></canvas>
	</div>

	<div id="buttons">
		<button id="snap">Snap</button><span> then </span> 
		<button id="send">Send</button>
		<span> or </span>
		<button id="snapAndSend">Snap and Send</button>
	</div>

	<div id="incoming">
		<h2>Incoming photo</h2>
		<div id="trail"></div>
	</div>

	<script src="/socket.io/socket.io.js"></script>
	<script src="js/lib/adapter.js"></script>
	<script src="js/main.js"></script>
</body>
```

이제 `localhost:8080`에서 확인할 수 있다.

> chrome 브라우저에서 `typeError`가 발생하는 경우가 있음. firefox로 접속하면 정상 작동


## 10. Congraturations

### Next steps
- Look at the code and architecture for the canonical WebRTC chat application AppRTC: [ `app` ](https://appr.tc/), [ `code` ](https://github.com/webrtc/apprtc).
- Try out the live demos from [ `github.com/webrtc/samples` ](https://codelabs.developers.google.com/codelabs/webrtc-web/#9).
