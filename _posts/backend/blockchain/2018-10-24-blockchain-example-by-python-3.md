---
layout: post
title:  "파이썬으로 블록체인 만들어보기 - 3"
categories: blockchain
---

# Blockchain example by python 3

이전 두 편에서는 파이썬으로 `Block` 클래스와 `Blockchain` 클래스를 실제로 만들어보았다. 보지 않았다면 미리 보고 오는것을 추천한다.

- [파이썬으로 블록체인 만들어보기 - 1. 기초 블록 만들기](http://seulcode.tistory.com/405)

- [파이썬으로 블록체인 만들어보기 - 2. 블록체인 클래스 만들기](http://seulcode.tistory.com/408)

`Block`, `Blockchain`의 전체 코드는 포스팅 맨 끝에 추가해두겠다.


## Blockchain 실제 사용해보기

이제 만들어진 블록체인으로 실제 나만의 블록체인을 만들어보자. `S-Coin`이라는 새로운 가상화폐를 만들어보겠다.


```python
from blockchain import Blockchain

scoin = Blockchain()
>>> <blockchain.Blockchain at 0x103da79b0>

# genesis block이 만들어진 것을 볼 수 있다.
print(scoin.chain)
>>> [<block.Block object at 0x103da35f8>]

# messi가 ronaldo에게 1000 코인을 주었다는 transaction1로
# 새로운 블록을 추가해보자

transaction1 = {
	'sender': 'Messi',
	'receiver': 'Ronaldo',
	'amount': 1000,
}

scoin.add_block(transaction1)
>>> ('00fcd4d5735ab741da6787a82d9c1e7093466a87086db5e9821331fcc2675be6',
>>> <block.Block at 0x103dd1d68>)

print(scoin.chain())
>>> [<block.Block at 0x103da35f8>, <block.Block at 0x103dd1d68>]

print(scoin.validate_chain())
>>> True

# transaction의 내용을 변경한 이후 validate가 사라지는지 확인해보자

last_block = scoin.chain[-1]
print(last_block.transactions)
>>> {'sender': 'Messi', 'receiver': 'Ronaldo', 'amount': 1000}

# 아까 추가한 1000 코인을 Ronaldo가 의도적으로 3000으로 늘려버리는 상황이다.
fake_transaction = {
	'sender': 'Messi',
	'receiver': 'Ronaldo',
	'amount': 3000,
}
last_block.transactions = fake_transaction

# 여기까지 보면 Ronaldo의 조작은 성공한 것으로 보인다.
print(last_block.transactions)
>>> {'sender': 'Messi', 'receiver': 'Ronaldo', 'amount': 3000}

# 하지만 validate을 해보면 해당 블록의 hash가 변경되어 False가 나오는것을 알수있다.
print(scoin.validate_chain())
>>> The current hash of the block does not equal the generated hash of the block.
>>> False
```

실제로 어렵지 않은 Block, Blockchain 두개의 클래스만으로도 기초적인 형태의 블록체인을 체험해 볼 수 있다. 

이처럼 블록체인은 어렵지 않은 기술이지만 그 가능성은 어마어마하다고 생각된다. 


### Block, Blockchain 클래스

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

    # prints contents of blockchain
    def print_blocks(self):
        for i in range(len(self.chain)):
            current_block = self.chain[i]
            print("Block {} {}".format(i, current_block))
            current_block.print_block()

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
