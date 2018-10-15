---
layout: post
title:  "MAC : 부트캠프 window 설치 되지 않을때 backup 지우기"
categories: mac
---


얼마 전 window를 사용해야 할 일이 있어 [부트캠프](https://support.apple.com/en-us/HT201468)를 설치를 하였다.

bootcamp는 맥에서 기본으로 제공하는 프로그램이기 때문에 큰 버그와 에러 없이 깔끔하게 하드디스크 파티션을 만들어서 윈도우를 설치할 수 있는 장점이 있다.

하지만 설치중에 끊임없이 용량 부족 등의 문제가 발생하였다. 처음에는 용량 자체를 줄이려고 시도하다가, local time machine snapshot이 큰 공간을 차지하여 진행이 안된다는 apple disscussion을 보았다.


- `Time Machine`에서 `자동으로 백업`을 비활성화 시킨다.
- `terminal` 앱에 들어가서 local snapshots들을 확인한다.

```
# 이 명령어를 입력하면 아래 존재하는 스냅샷들이 나온다.
$ tmutil listlocalsnapshots /
com.apple.TimeMachine.2017-12-14-173102
com.apple.TimeMachine.2017-12-14-212356
com.apple.TimeMachine.2017-12-15-052254
```

- 필요 없는 스냇샷 (보통은 전부 지워주면 원활하게 부트캠프가 설치된다)을 지워준다. 지울때에는 `tmutil deletelocalsnapshots <snapshot_date>` 명령어로 지울 수 있다.

```
$ tmutil deletelocalsnapshots 2017-12-14-173102
```
