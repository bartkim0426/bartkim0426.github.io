---
layout: post
title:  "파이썬으로 블록체인 만들어보기 - 2"
categories: blockchain
---

# Blockchain example by python 2

## Blockchain 만들기

전편에서 파이썬으로 간단한 블록을 만들어 보았다. 

아직 안 보신 분들은 이전 글인 [파이썬으로 블록체인 만들어보기 1. 기초 블록 만들기](http://seulcode.tistory.com/405)를 보고 오는것을 추천한다.

이번에는 이전에 만든 블록을 가지고 실제로 이전 블록과 다음 블록을 연결한 `BlockChain`을 만들어 보겠다.

우선 블록체인 클래스는 어떤 값과 메소드를 가져야 할지 생각해보자

- 모든 블록들의 list
- 모든 transaction들
- `genesis_block`을 만드는 메소드
- 블록을 추가하는 메소드
- chain validation

### `init`

블록체인 클래스에서 처음에 필요한 항목들을 `__init__`으로 초기화 해준다. 모든 블록을 저장할 `chain` list와 모든 transaction 데이터를 저장할 `all_transactions`를 추가해준다.

```python
from block import Block

class Blockchain:
    def __init__(self):
        self.chain = []
        self.all_transactions = []
```


### genesis block
우선 `genesis block`을 생성하는 메소드를 추가해보자. `genesis block`이란 블록 체인의 첫 블록으로 이전 블록을 갖지 않는 유일한 블록이다.

genesis block은 transactions (거래내역)과 이전 해시 값을 가질 필요가 없으므로 `Block({}, 0)`으로 생성이 가능하다.

그리고 클래스가 사용될 때 바로 genesis block이 생성되도록 `__init__` 안에 `genesis_block()`을 실행시킨다.

```python
from block import Block

class Blockchain:
    def __init__(self):
        self.chain = []
        self.all_transactions = []
        self.genesis_block()

    def genesis_block(self):
        transactions = {}
        block = Block(transactions, 0)
        self.chain.append(block)
        return self.chain
```


### POW (`proof of work`)
새로운 블록을 추가하기 위해 블록체인의 작업증명 함수를 추가해보자. 원래는 새로운 블록이 추가될 때 block의 difficulty에 따라서 nonce값을 증가시키며 해당 범위에 있는 hash 값을 찾는 작업이 필요하다. 이번 예제에서는 이를 내부 함수로 만들어서 자동으로 POW가 진행되도록 작성해보겠다.

`proof_of_work`는 difficulty (기본 2)에 따라서 해당 hash를 찾기 위해 계속 nonce값을 더하면서 맞는 값을 찾아낸다.

예를들면 `difficulty`가 2인 경우 (`proof_of_work(block, difficulty=2)`) hash의 처음 두자리가 `00`으로 시작되는 해쉬값을 찾기 위해 계속 nonce를 더해간다.

```python
from block import Block

class Blockchain:
    def __init__(self):
        self.chain = []
        self.all_transactions = []
        self.genesis_block()

    def genesis_block(self):
        transactions = {}
        block = Block(transactions, 0)
        self.chain.append(block)
        return self.chain

    def proof_of_work(self, block, difficulty=2):
        proof = block.generate_hash()

        while proof[:2] != '0'*difficulty:
            block.nonce += 1
            proof = block.generate_hash()
        return proof
```

### block 추가하기

이제 새로운 블록이 추가되는 `add_block` 메소드를 추가해주자. genesis block과는 다르게 이후 블록들은 transactions, 이전 hash 값을 가진다.

```python
from block import Block

class Blockchain:
    def __init__(self):
        self.chain = []
        self.all_transactions = []
        self.genesis_block()

    def genesis_block(self):
        transactions = {}
        block = Block(transactions, 0)
        self.chain.append(block)
        return self.chain

    def add_block(self, transactions):
        previous_block_hash = self.chain[len(self.chain)-1].hash
        new_block = Block(transactions, previous_block_hash)
        proof = self.proof_of_work(new_block)
        self.chain.append(new_block)
        return proof, new_block

    def proof_of_work(self, block, difficulty=2):
        proof = block.generate_hash()

        while proof[:2] != '0'*difficulty:
            block.nonce += 1
            proof = block.generate_hash()
        return proof
```

### Validate

이제 초기 형태의 블록체인이 완성되었다! 

하지만 지금은 validation이 안되기 때문에 블록의 정보를 변경하더라도 해당 블록체인이 validate 한지 알 수 없다.

간단한 `validate_chain` 메소드를 만들어 해당 블록체인의 위변조가 없었는지 (hash값이 변경되지 않았는지) 확인해보자.

```python
from block import Block


class Blockchain:
    def __init__(self):
        self.chain = []
        self.all_transactions = []
        self.genesis_block()
    
    def genesis_block(self):
        transactions = {}
        block = Block(transactions, 0)
        self.chain.append(block)
        return self.chain

    def add_block(self, transactions):
        previous_block_hash = self.chain[len(self.chain)-1].hash
        new_block = Block(transactions, previous_block_hash)
        proof = self.proof_of_work(new_block)
        self.chain.append(new_block)
        return proof, new_block

    def validate_chain(self):
        for i in range(1, len(self.chain)):
            current = self.chain[i]
            previous = self.chain[i-1]
            if current.hash != current.generate_hash():
                print("The current hash of the block does not equal the generated hash of the block.")
                return False
            if previous.hash != previous.generate_hash():
                print("The previous block's hash does not equal the previous hash value stored in the current block.")
                return False
            if current.previous_hash != previous.hash:
                return False
        return True
  
    def proof_of_work(self, block, difficulty=2):
        proof = block.generate_hash()

        while proof[:2] != '0'*difficulty:
            block.nonce += 1
            proof = block.generate_hash()
        return proof
```


이제 기초적인 형태의 blockchain이 완성되었다!

다음 시간에는 만들어진 `Block`과 `Blockchain`을 사용해서 실제로 나만의 블록체인을 파이썬으로 구현해보겠다.
