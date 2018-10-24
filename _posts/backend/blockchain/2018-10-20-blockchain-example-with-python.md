---
layout: post
title:  "파이썬으로 블록체인 만들어보기 - 1"
categories: blockchain
---


온라인 강의 플랫폼 `codecademy`에 블록체인 코스가 생겼다. (물론 모두 하려면 `Pro`로 계정을 업그레이드 해야하지만 현재 `Pro` 무료 이벤트를 진행중이기 때문에 3일정도 `Pro` 버전을 사용해볼 수 있다.)


blockchain이란 무엇인지에 대한 기초적인 설명부터 `POW`(Proof of work), `mining` 등에 대해서 잘 설명해준다. (text 위주이기 때문에 초반에는 조금 지루할 수 있다.)

이후에는 `python`으로 기초적은 수준의 `Block`과 `Blockchain`을 직접 만들어볼 수 있다.

이번 편에서는 파이썬으로 `Block`을 만드는 예시를 써보았다.


# Blockchain example by python

## Block 만들기

우선 `Block`은 말그대로 하나의 블록을 의미한다. 각각의 블록은 
- `timestamp`: 언제 만들어졌는지,
- `hash`: 현재 블록의 해시값
- `previous hash`: 이전 블록의 해시값
- `nonce`: 논스
- `transaction`: 실제 데이터 정보인 트랜젝션

의 데이터값을 가진다. 이를 블록 클래스로 표현하면 다음과 같다.

```python
class Block:
    def __init__(self, transactions, previous_hash, nonce = 0):
		# datetime.now()를 사용해서 현재 값을 timestamp
        self.timestamp = datetime.now()
        self.transactions = transactions
        self.previous_hash = previous_hash
        self.nonce = nonce
		# generate_hash() 함수를 만들어서 해시를 생성
        self.hash = self.generate_hash()
```

그럼 실제로 해시를 만드는 `generate_hash()` 함수를 만들어보자.

파이썬에서는 `hashlib`를 활용해서 `sha256`을 쉽게 구현할 수 있다.

해시처리하고싶은 정보를 `sha256` 안에 넣고 (string이라면 `encode()`를 해주어야한다) 이를 `hexdigest` 함수를 사용하여 프린트할 수 있다.

```python
from hashlib import sha256

test_content = 'test string for making hash'
test_hash = sha256(test_content.encode())
print(test_hash.hexdigest())
```

이를 활용하여 실제로 `Block` 클래스의 `generate_hash()` 함수를 만들어 보면 다음과 같다.

```python
from datetime import datetime
from hashlib import sha256

class Block:
    def __init__(self, transactions, previous_hash, nonce = 0):
        self.timestamp = datetime.now()
        self.transactions = transactions
        self.previous_hash = previous_hash
        self.nonce = nonce
        self.hash = self.generate_hash()

    def print_block(self):
        # prints block contents
        print("timestamp:", self.timestamp)
        print("transactions:", self.transactions)
        print("current hash:", self.generate_hash())

    def generate_hash(self):
        # hash the blocks contents
        block_contents = str(self.timestamp) + str(self.transactions) + str(self.previous_hash) + str(self.nonce)
        block_hash = sha256(block_contents.encode())
        return block_hash.hexdigest()
```


다음 편에서는 실제 이 블록들을 활용한 `Blockchain` 클래스를 만들고 이를 실제로 활용해볼 것이다.
