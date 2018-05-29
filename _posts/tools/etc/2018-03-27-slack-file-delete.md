---
layout: post 
title:  "슬랙(slack) 파일 용량 비우기 (파일 지우기)"
categories: etc
---


우리 회사에서는 업무 커뮤니케이션 방법으로 `Slack`, `Trello`를 사용한다.

둘 다 기본적인 기능들이 무료로 제공되고, 매우 잘 만들어진 앱들이기 때문에 아주 만족하면서 사용중이다.


그러던 중, 어느 순간부터 슬랙에 파일을 업로드 할 때 사용 가능한 용량이 얼마 남지 않았다는 메세지가 뜨기 시작했고... 무료 사용 한도를 넘어서 버리자 더 이상 파일이 올라가지 않게 되었다.

물론 유료로 전환해서 사용해도 되지만, 우리가 공유하는 파일들은 대부분 일회성의 파일이 많았고 이를 그대로 가지고 있기보다는 주기적으로 비워주면서 5G를 사용해도 충분할 거라고 생각해서 파일을 지워 보았다.


그냥 파일을 지웠더니 역시나 용량 자체가 줄어들지는 않기에, 해당 파일을 지워주는 방법을 찾다가 간편하게 지워주는 코드를 작성해 놓아서 가져와서 조금 다듬어서 사용하게 되었다. 


내 깃헙 ([여기](https://github.com/bartkim0426/slack-files-clear))에서 소스를 다운받을 수 있으며, 다운 받은 뒤 `python main.py`로 쉽게 실행시키면 된다.

사용법은 아주 간단하다.

```bash
$python main.py
Enter your slack token (If you dont know, check https://api.slack.com/custom-integrations/legacy-tokens):'xoxo-example-input-xxx'
Please enter days that you want to delete: 
```
token 입력을 요구하면 해당 토큰을 따움표(`''`)로 싸서 넣어준다. (ex. `'xoxo-18xxxx-xxxx.....'`)

그리고 파일을 지우고자 하는 날짜를 입력해준다.

많은 사람들이 유용하게 사용했으면 좋겠다.


> 현재는 python 2 버전으로 되어있어서 python2로 실행시켜야 한다. 추후 3으로 리팩토링 할 예정.
