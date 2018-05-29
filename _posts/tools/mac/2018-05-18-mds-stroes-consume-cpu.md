---
layout: post
title:  "MAC : mds_store cpu 점유율 문제"
categories: mac
---


요즘 맥북 프로 (2017, 13, touchbar) 모델이 종종 무지개가 뜨고 cpu 점유율이 100에 가깝게 유지되는 현상이 지속적으로 발생해서, 확인해보니 `mds_store`가 상당히 큰 cpu/memory를 잡아먹고 있었다.

확인해보니 spotlight 활동과 관련된 것인데, 나는 spotlight search를 전혀 쓰지 않고 `alfred`를 사용하기 때문에 스포트라이트 검색 인덱싱을 비활성화 처리하니 문제가 해결되었다.


터미널에서 명령어 한줄로 쉽게 비활성화 처리할 수 있다. (`terminal` 실행 후)

```
sudo mdutil -a -i off
```

다시 활성화 하려면 `on`을 해주면 된다.

```
sudo mdutil -a -i on
```
