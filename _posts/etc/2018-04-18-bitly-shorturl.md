---
layout: post
title:  "bitly shorturl 사용하기"
categories: etc
---


2018년 4월부터 구글 shortern url 의 서비스가 종료되어서 해당 api를 사용하는 프로젝트를 급하게 수정할 일이 생겼다.

그래서 찾던 중 `Bitly` 라는 서비스가 있어서 사용해보았다.


개인 계정으로는 10,000개까지 무료로 사용이 가능하다.

우선 [bitly 홈페이지](bitly.com)에서 회원가입을 하면 [콘솔창](app.bitly.com)으로 이동된다.

오른쪽 메뉴 - profile 을 들어가면 `Generic Access Token`이 있는데, 여기서 비밀번호를 입력한 후 토큰을 발급받아 이를 적어둔다.


bitly 콘솔을 사용해서 직접 url을 줄일수도 있지만, `api`를 통해서 코드로 줄이는 일이 많을텐데 [bitly documentation](https://dev.bitly.com/transition_from_google.html)에서 확인할 수 있따.

사용법은 아주 간단하다. 


`http://google.com`을 줄이려면 다음 request를 `GET`방식으로 보내면 된다.

```
GET https://api-ssl.bitly.com/v3/shorten?access_token=XXXX&longUrl=http%3F%2A%2Agoogle.com%2A
```

파이썬 `requests`라이브러리를 활용하면 다음과 같다.

```python
import json
import requests

# YOUR_BITLY_ACCESS_TOKEN 대신 위에서 발급받은 토큰을 입력해준다.
# 200(success)코드가 아닌 경우에는 기존 url을 반환한다.
def bitly_short_url(url):
    post_url = 'https://api-ssl.bitly.com/v3/shorten?access_token={token}&longUrl={url}'.format(
        token=YOUR_BITLY_ACCESS_TOKEN,
        url=url
    )
    res = requests.get(post_url)
    if res.status_code == 200:
        return res.json().get('data').get('url')
    else:
        return url
```

이외에 link 히스토리 보기, 분석 등도 `api`를 통해서 확인할 수 있는데 자세한 내용은 [dev.bityl.com](https://dev.bitly.com/transition_from_google.html) 참고
