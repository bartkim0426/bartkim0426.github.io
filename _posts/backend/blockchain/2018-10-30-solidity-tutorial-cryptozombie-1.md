---
layout: post
title:  "솔리디티 크립토좀비 튜토리얼 - 1. 스마트계약 만들기"
categories: blockchain
---

## Solidity란?

- 솔리디티는 이더리움 블록체인 플랫폼에서 스마트 계약을 정의하는 언어.
- Javascript와 유사하지만 정적 타입 언어이기 때문에 자료형 명시해주어야함 (Java와 비슷)
- 자세한 내용은 [위키백과]([https://ko.wikipedia.org/wiki/솔리디티](https://ko.wikipedia.org/wiki/%EC%86%94%EB%A6%AC%EB%94%94%ED%8B%B0)) 참고

솔리디티 코드는 `컨트랙트` 안에 싸여있음. 

> 컨트랙트는 이더리움 애플리케이션의 기본적인 구성 요소로, 모든 변수와 함수는 어느 한 컨트랙트에 속하기 마련이다.

```javascript
contract HelloWorld {
}
```

## Version Pragma

모든 솔리디티 소스코드는 version pragma로 시작해야함. 버전 선언. 

즉, 모든 코드는 다음과 같이 시작된다.


```javascript
pragma solidity ^0.4.19;
contract HelloWorld {
}
```

## State variable & Integers

`State variables` (상태 변수)는 contract 저장소에 저장된다. 즉, 이더리움 블록체인에 작성된다는 것. 

솔리디티는 정적 타입 언어이기 때문에 자료형을 명시해주어야한다. 

`unit` : 부호 없는 정수, 즉 값이 음수가 아니여야함. (부호 있는 정수는 `int`)

- 참고 : 솔리디티에서 `uint`는 실제로는 `uint256`, 즉 256비트 정수의 표현.

## Structs

솔리디티는 더 복잡한 자료 타입을 위해서 `Structs`(구조체)를 제공. (javascript의 object와 비슷하다고 생각하면 될듯하다)


```javascript
struct Person {
	uint age;
	string name;
}
```

## Arrays

솔리디티의 배열은 `fixed arrays` 와 `dynamic arrays`로 나뉜다. (정적 배열, 동적 배열)

- 정적 배열(고정배열)은 한정된 원소만을 담을 수 있다. ex) `uint[2] fixedArray;`


```javascript
// 정적배열 예시
uint[2] fixedArray;
string[5] stringArray;
// 동적배열은 고정된 크기가 없다
uint[] dynamicArray;
// structs의 배열도 생성 가능
Person[] people;
```

## public

배열 선언시 `public`으로 선언하면 다른 컨트랜트들이 이 배열을 읽을 수 있음 ⇒ 공개 데이터를 저장할 때 유용하게 쓰임

    Person[] public people;

## 함수 선언

솔라디티에서 함수 선언은 다음과 같다. javascript와 흡사하지만 전달하는 인자의 자료형을 명시해주어야하고, 인자명을 `_`로 시작해서 전역 변수와 구분 (관례)

```javascript
function eatHamburgers(string _name, uint _amount) {
}
```

## Working with structs and arrays

sturcts는 일반적인 객체 형식으로 사용이 가능하고, `push`를 사용해서 배열에 값을 추가 가능

```javascript
struct Person {
	uint age;
	string name;
}

Person[] public people;

Person satoshi = Person(172, "Satoshi");
people.push(satoshi);
// 한줄로 표현 가능하다
people.push(Person(172, "Satoshi"));
```

## Private / Public

솔리디티에서, 함수는 기본적으로 `public`으로 선언된다. 이는 다른 contract의 누구나 해당 함수를 확인하고 실행할 수 있다는 것. 

이는 블록체인의 공개성의 취지와는 맞지만, contract를 공격에 취약하게 만들 수 있기 때문에 기본적으로 private으로 선언하고 공개할 함수만 public으로 선언하는 것이 좋다.

private 함수를 선언하려면 `private`을 붙여주면 된다. 함수 인자명과 마찬가지로 private 함수명도 `underscore`  (`_`)로 시작하는게 관례


```javascript
uint[] unmbers;

function _addToArray(uint _number) private {
	numbers.push(_number);
}
```

## Function return

함수에서 특정 값을 리턴받으려면 반환값 종류를 포함해서 선언해야한다. 

```javascript
string greeting = "What's up dog";

function sayHello() public returns (string) {
	return greeting;
}
```

## Function modifiers

**view** 함수

위의 예시 함수 `sayHello()`는 솔리디티에서 상태를 변화시키지 않음. 즉, 어떤 값을 변경하거나 writing 하지 않음. ⇒ 이 경우 함수를 `view` 함수로 선언. (데이터를 보기만 하고 변경하지 않는다)

```javascript
function sayHello() public view returns (string) {
	return greeting;
}
```

**pure 함수**

함수가 앱에서 어떤 데이터에도 접근하지 않고 (읽지도 않고) 반환값이 함수에 전달된 인자값에 따라서 달라지는 경우 `pure` 로 선언

```javascript
function _multiplyPure(uint a uint b) private pure returns (uint) {
	return a * b;
}
```

## keccak256

이더리움은 `SHA3`의 한 버전인 `keccak256`을 내장 해시 함수로 가지고 있음. 

조금만 input이 달라져도 반환값은 완전히 달라질 수 있다.

```javascript
keccak256("aaaab");
//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
keccak256("aaaac");
//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
```

## 이벤트

이벤트는 contract가 블록체인의 사용자단에서 액션이 발생했을 때 커뮤니케이션 하는 방법. 

컨트랙트는 특정 이벤트가 일어나는지 귀를 기울이다가 해당 이벤트가 발생하면 행동.

```javascript
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public {
	uint result = _x + _y;
	// 이벤트를 실행시켜서 add 함수가 실행되었음을 알린다
	IntegersAdded(_x, _y, result);
	return result;
}
```

이를 `javascript`로 구현하면 다음과 같음 

```javascript
YourContract.IntegersAdded(function(error, result) {
})
```

## Web3.js

지금까지 솔리디티 컨트랙트를 만들어보았다. 이제 이 컨트랙트와 상호작용하는 client단의 자바스크립트 코드를 작성해야함. 

이더리움은 `web3.js` 라이브러리를 가지고 있다.

