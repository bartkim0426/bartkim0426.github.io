---
layout: post
title:  "cron: 기본 사용법"
categories: linux
---

`crontab`은 특정 시간에 특정 작업을 하는 리눅스의 스케쥴러이다.


### 기본 사용법

기본 명령어는 다으과 같다.
```bash
# 현재 크론 리스트 보기
crontab -l

# 크론탭 명령어 추가하기
crontab -e

# 크론탭 삭제
crontab -d
```

### 크론탭 주기(`* * * * *`)
크론탭은 `*` 5개로 주기를 설정 가능하다.
```
*   *   *   *   *
min hour day month date(요일)
```
위 5개의 `*`에 특정 시간(날짜)를 조합하여 크론탭을 설정 가능하다. 매번 사용하려면 `*` 그대로 남겨두면 된다.

다음은 매분 `ls -al`을 실행하는 예시이다.
```
* * * * * ls -al
```

간단한 예시를 보자. 매일 오전 10시에 작업을 하려면 다음과 같이 작업하면 된다.
```
# 오전 10시
0 10 * * * ls -al

# 매주 금요일 오후 10시 30분
30 22 * * 5 ls -al
```

간격도 설정이 가능하다. `*/20`은 20분마다, `*/5`는 5분마다이다.

```
# 매 10분마다 실행
*/10 * * * * ls -al
```

또한 특정 범위에서만 작동시킬 수도 있다.
```
# 매일 10시 0 - 10 분동안 매분 실행
0-10 10 * * * ls -al
```

즉, 5개의 시간 (분, 시간, 일, 월, 날짜)에 `*`, `-`, `/` 세가지 명령어를 조합해서 다양한 케이스에 사용이 가능하다.
