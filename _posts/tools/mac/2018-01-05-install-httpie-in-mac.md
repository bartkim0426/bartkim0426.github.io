---
layout: post
title:  "맥 (os X)에 httpie 설치하기 및 사용법"
categories: mac
tags: mac httpie
---

`django rest framework`를 사용하면서 API에 GET, PUT, POST 등의 request를 보내는 일이 잦아졌다. 브라우저에서 보내는 데에는 한계가 있고, postman을 사용하자니 조금 귀찮아서 터미널에서 보낼 수 있는 방법을 찾아보던 중 `httpie`를 알게 되었다.

기존에 `curl`로 복잡하게 해야 하던 것을 훨씬 더 쉽게 할 수 있다. 

자세한 내용은 [httpie 공식 페이지](https://httpie.org) 참고. [여기](https://httpie.org/run)를 들어가면 httpd를 테스트 해볼 수 있다.


## `httpie` 설치하기
우선 mac 에서 `httpie`는 `brew`를 사용해서 간단하게 설치 할 수 있다. 

```bash
$brew install httpie
```

## `httpie` 사용법
`http` 명령어를 사용해서 직관적으로 사용이 가능하다.


우선 `http GET`을 사용해서 간단하게 get request를 받을 수 있다. 

결과로는 http status와 header, body 등 다양한 정보를 볼 수 있다.
```bash
$http GET localhost:8000/api/jobs

HTTP/1.0 200 OK
Allow: GET, PUT, PATCH, DELETE, HEAD, OPTIONS
Content-Length: 208
Content-Type: application/json
Date: Fri, 05 Jan 2018 09:03:16 GMT
Server: WSGIServer/0.2 CPython/3.6.0
Vary: Accept, Cookie
X-Frame-Options: SAMEORIGIN

{
    "apply_to_email": "",
    "apply_to_url": "",
    "created_at": "2017-12-01T17:16:29.763700+09:00",
    "deadline": "2017-12-01",
    "id": 1,
    "job_type": "FULL",
    "logo": null,
    "slug": "",
    "start_date": "2017-12-01",
    "title": "test",
    "user": 3
}
```

