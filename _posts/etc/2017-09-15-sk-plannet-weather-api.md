---
layout: post
title: "SK-plannet-weather-api 사용하기"
categories: api
tags: api weather
---

### sk plannet 가입 후 api key 발급받기
[sk plannet 개발자 센터](https://developers.skplanetx.com/)에 회원가입 후 앱 생성 페이지에 가서 weather api를 체크하고 앱을 생성하면 api key가 발급된다

### weather api request 보내기
[weather api](https://developers.skplanetx.com/apidoc/kor/weather/information/#doc1168)를 보면 분별 현재 날씨, 시간별 현재 날씨 이외에도 다양한 API들이 있다. 이 중 원하는 api를 선택한 후 header에 `appKey`로 발급받은 API key를 넘겨서 api를 호출하면 결과를 받을 수 있다. 

나는 분별 날씨로 테스트를 하였다.

`http://apis.skplanetx.com/weather/current/minutely?version={version}&lat={lat}&lon={lon}&city={city}&county={county}&village={village}&stnid={stnid}` 위 url로 GET request를 보내면 되는데, 

1. lat/lon (위도/경도)
2. city/country/village (도시-구-동 ex- 서울시 강남구 도곡)
3. stnid (관측소 id: 서울-108, [타 도시 관측소 id 확인하기](http://minwon.kma.go.kr/main/obvStn.do))
위 셋 중에 하나만 선택해서 파라미터로 보내주면 된다. 서울 지역 stnid로 request를 보내면, 다음과 같다.

```python
import os
import json
import requests
# 고려대역 37.590498, 127.035831
# 럭키아파트 37.559292, 126.948914
# 설입 37.486606, 126.952817
# 전주 35.811665, 127.108352
# 화순 34.984891, 126.961802
lat_lon = {
	"ku": ["37.590498", "127.035831"],
	"lucky": ["37.559292", "126.948914"],
	"snu": ["37.486606", "126.952817"],
	"jeonju": ["35.811665", "127.108352"],
	"hwasun": ["34.984891", "126.961802"],
}

location = "ku"
params = {"version": "1", "lat": lat_lon[location][0], "lon": lat_lon[location][1]}
# your_api_key에 발급받은 api key를 넣어준다
headers = {"appKey": os.environ["SK_PLANNET_WEATHER_API_KEY"]}
res = requests.get("http://apis.skplanetx.com/weather/current/minutely", params=params, headers=headers)
data = res.json()
weather = data.get('weather').get('minutely')[0]
```
