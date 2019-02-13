---
layout: post
title:  "IPython (Jupyter) notebook에서 unittest 하기"
categories: python
---


요즈음 대부분의 코드에서 테스트를 작성하려고 노력중이다.

하지만 jupyter notebook으로 코드를 작성하면서 테스트를 작성하자 아래 에러가 나타났다.


```python
if __name__ == '__main__':
    unittest.main()
```


```
E
======================================================================
ERROR: /Users/path_to_kernel (unittest.loader._FailedTest)
----------------------------------------------------------------------
AttributeError: module '__main__' has no attribute '/Users/path_to_kernel'

----------------------------------------------------------------------
Ran 1 test in 0.001s

FAILED (errors=1)
```

그 이유는 jupyter의 커널 명이 `sys.argv`의 첫 파라미터로 `unittest.main`에 전달되기 때문이다.

이럴때 다음과 같이 작성하면 테스트가 성공하는것을 볼수있따.

```python
if __name__ == '__main__':
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
)
```


다음은 테스트 예시

![test image](https://i.imgur.com/BKG7PyQ.png){:class="img-responsive"}
