---
layout: post 
title:  "내가 보려고 정리하는 파이썬 테스트(unittest)"
categories: python
---

그동안 테스트를 전혀 짜지 않은 것은 아니지만, 프로젝트가 복잡해질수록 테스트를 잘 짜지 않았었다. 그러던 중 테스트 코드의 필요성을 점점 느끼게 되면서, 그동안 어깨넘어로만 써오던 파이썬 테스트 코드를 한번 기초부터 정리해보고 싶었다.

그래서 이번 기회에 django 프로젝트가 아니라 test code를 만들면서 그냥 python module을 만들어 정리해보았다.


### 프로젝트 구성
우선 프로젝트 구성은 다음과 같다. 간단한 `blog`, `posts`, `api` 세 개의 파일과 `tests` 디렉토리로 구성.   
tests/는 독립적인 테스트를 진행하는 `unit`, 여러 object에 연관된 테스트를 모아논 `integration`으로 나누었따. (테스트 구성은 한 폴더에 넣어도 되고, 그냥 개인의 취향에 맞게 나누면 될 듯 하다.)
```
> blog/
--> tests/
-->--> __init__.py
-->--> integration/
-->-->--> __init__.py
-->--> unit/
-->-->--> __init__.py
--> blog.py
--> posts.py
--> api.py
```

### 첫 테스트 코드
파이썬에서는 테스트를 위해서 `unittest` 모듈을 제공한다. `unittest.TestCase`를 상속받은 테스트 클래스를 만들고, 해당 클래스 안에 `test_`로 시작하는 함수를 만들면 unittest가 해당 테스트(함수)를 테스트해준다. (`test_`로 시작하는 함수명은 바꿀 수 있다.)   

해당 test 메소드는 `self.assertEqual` 등 많은 assert 함수들을 가지고 있다. 이를 활용해서 아주 쉽게 테스트를 해볼 수 있다.   

일단 `1+1=2`인지를 확인하는 테스트 코드를 만들어보자.
```python
from unittest import TestCase

class PostTest(TestCase):
	def test_post(self):
		self.assertEqual(1+1, 2)
```

테스트는 terminal에서 `python -m unittest` 명령어로 쉽게 실행시킬 수 있다.   

나처럼 모듈로 쪼개서 테스트를 만들었다면, `tests`의 `__init__.py`에서 해당 테스트 클래스를 호출해 주어야 한다.
#### `__init__.py`
```python
from unit.post_test import PostTest
# 한번에 다 불러오려면 *을 사용해도 무방하나 권장하지 않음
# from unit.post_test import *
```

### 테스트 실행
`python -m unittest`를 실행시키면 테스트 결과가 다음과 같이 나온다. 테스트 클라스 내의 `def test_...`로 시작하는 함수 한개당 한개의 테스트로 인식한다.

```bsah
$python -m unittest

.
----------------------------------------------------------------------
Ran 1 tests in 0.001s

OK
```

> 파이참을 이용하면 훨씬 더 쉽게 테스트를 실행시킬 수 있다. 나는 파이참을 사용하지 않아서 그냥 termianl에서 테스트를 실행시켰지만, 파이썬은 테스트를 작성하면서 해당 테스트를 바로 실행시켜볼 수도 있고, 전체 테스트를 실행시킬 수도 있다.


