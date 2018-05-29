---
layout: post
title:  "파이썬에서 실행 시간 체크하기 (check executed time in python)"
categories: python
tags: python backend tip
---

파이썬에서 몇만개가 되는 쿼리를 순회하는 등 시간이 오래 걸리는 작업을 하면 항상 얼마가 걸렸는지 정확히 파악하기 어렵다. 그래서 executed time을 체크하는 방법을 찾아보았다.


방법은 매우 간단. 파이썬 내장 함수인 `time`을 사용하면 된다.

```python
import time

# start_time을 체크
start_time = time.time()
for idx, a in enumerate(lists):
    if idx % 1000 == 0:
        print(idx)
	do_sth_for_a(a)
# 마지막에 start time을 뺀 값을 프린트
print("---{}s seconds---".format(time.time()-start_time))
```

> 몇만개가 넘는 쿼리를 순회하는 경우 잘 진행되고 있는지 확인하기 위해서 enumerate로 index를 프린트 하는 방법도 좋은 생각인 것 같다. 위에서는 1000단위로 프린트가 되어서 진행 상태를 대략적으로 확인 가능하다.
