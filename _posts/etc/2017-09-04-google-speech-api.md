---
layout: post
title:  "using goole speech API"
categories: api
---

### Join Google Cloud Platform
지금 가입하면 1년동안 300$ 무상 크레딧을 제공하는 cloud platform에 [가입](https://console.cloud.google.com/freetrial)한다. 가입시 카드(visa, master 등)를 등록해야 평가판 사용이 가능하다. (유료 전환하지 않으면 결제는 되지 않는다.)

### Making Project
speech를 사용할 프로젝트를 만들고 speech api를 [활성화](https://console.cloud.google.com/flows/enableapi?apiid=speech.googleapis.com) 시킨다. 

### Install Cloud SDK
인증을 위해서 gcloud 커맨드를 설치해야 한다. 이 부분은 비개발자라면 조금 까다로울 수 있는데.. 각자의 운영체제에 맞춰서 [공식 문서](https://cloud.google.com/sdk/) 를 잘 따라하기만 하면 설치 할 수 있으므로 따라서 설치를 해준다.

### Create credential file
Speech API를 활성화 한 후 사용자 인증 정보로 이동한 뒤, 쭉 따라하면 JSON 파일 (service account key file)이 자동으로 다운로드 된다. 이를 적당한 위치에 저장한 후 환경변수에 추가해준다.

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/secret-key.json
```

### Recording as Flac format
구글 API에서 인식하는 인코딩 포멧을 맞춰야 하는데, 나중에 변환하는 것보다 처음부터 해당 포멧으로 녹음 하는 것이 좋다. Flac을 지원하는 [audacity](https://cloud.google.com/sdk/) 등의 무료 녹음 프로그램으로 녹음하면 되겠다.

### Upload to google storage

### Making json file
한글을 사용하고 싶으면 `"languageCode": "ko-KR"`로 변경해준다.   
url에 해당 파일의 cloud 주소 url을 넣어준다.    

```json
{
  "config": {
      "encoding":"FLAC",
      "sampleRateHertz": 16000,
      "languageCode": "en-US",
      "enableWordTimeOffsets": false
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```


### Call by curl
위에서 만든 json 파일을 curl 로 호출하면 결과값을 얻을 수 있다.   
```
curl -s -k -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ACCESS_TOKEN \
    'https://speech.googleapis.com/v1/speech:recognize' \
	  -d @request.json
```


> 위의 내용대로 하면 한 파일의 결과값을 얻어내기는 쉽지만, 여러 가지 파일을 주기적으로 결과값을 받아내기가 쉽지 않다. 따라서 python으로 쉽게 해당 파일의 결과를 얻는 코드를 작성해 볼 예정이다. 우선 python-doc-sample에서 제공하는 speech 예시로 특정 파일의 결과값을 쉽게 얻어낼 수 있다.


### using python-sample

download [python-docs-sample/speech](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/speech/cloud-client) and follow README

### speec-to-server
누군가 이미 만들어 놓은 client side [라이브러리](https://github.com/akrennmair/speech-to-server)를 쓰면 web상에서 녹음을 해 바로 google speech api로 보낼 수 있다.
