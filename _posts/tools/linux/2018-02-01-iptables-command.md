---
layout: post
title:  "리눅스 방화벽 설정 - iptables"
categories: linux
---

## iptables란?
"iptables는 시스템 관리자가 리눅스 커널 방화벽(다른 넷필터 모듈로 구현됨)이 제공하는 테이블들과 그것을 저장하는 체인, 규칙들을 구성할 수 있게 해주는 사용자 공간 응용 프로그램이다." - [위키백과 iptables](https://ko.wikipedia.org/wiki/Iptables)


새로 서버를 구축할 때 iptables 설정으로 여러 번 골머리를 앓다가 한 번 정리를 해 두면 두고두고 좋을 것 같아서 정리 해 본다.

iptables에 대한 기초 설명은 잘 정리된 곳에서 참조했다. ([참고 블로그](http://webdir.tistory.com/170))

### 1) 테이블(tables)
우선 iptables에는 테이블이라는 광범위한 범주가 있는데, 이 테이블은 filter, nat, mangle, raw 같은 4개의 테이블로 구성되며, 이중에서 우리에게 필요한 것은 필터링 규칙을 세우는 filter 테이블이다.

### 2) 체인(chain)
iptables에는 filter 테이블에 미리 정의된 세가지의 체인이 존재하는데 이는 INPUT, OUTPUT, FORWARD 이다. 이 체인들은 어떠한 네트워크 트래픽(IP 패킷)에 대하여 정해진 규칙들을 수행한다.

가령 들어오는 패킷(INPUT)에 대하여 허용(ACCEPT)할 것인지, 거부(REJECT)할 것인지, 버릴(DROP)것인지를 결정한다.

- INPUT : 호스트 컴퓨터를 향한 모든 패킷
- OUTPUT : 호스트 컴퓨터에서 발생하는 모든 패킷
- FORWARD : 호스트 컴퓨터가 목적지가 아닌 모든 패킷, 즉 라우터로 사용되는 호스트 컴퓨터를 통과하는 패킷

### 3) 매치(match)
iptables에서 패킷을 처리할때 만족해야 하는 조건을 가리킨다. 즉, 이 조건을 만족시키는 패킷들만 규칙을 적용한다.

- --source (-s) : 출발지 IP주소나 네트워크와의 매칭
- --destination (-d) : 목적지 ip주소나 네트워크와의 매칭
- --protocol (-p) : 특정 프로토콜과의 매칭
- --in-interface (i) : 입력 인테페이스
- --out-interface (-o) : 출력 인터페이스
- --state : 연결 상태와의 매칭
- --string : 애플리케이션 계층 데이터 바이트 순서와의 매칭
- --comment : 커널 메모리 내의 규칙과 연계되는 최대 256바이트 주석
- --syn (-y) : SYN 패킷을 허용하지 않는다.
- --fragment (-f) : 두 번째 이후의 조각에 대해서 규칙을 명시한다.
- --table (-t) : 처리될 테이블
- --jump (-j) : 규칙에 맞는 패킷을 어떻게 처리할 것인가를 명시한다.
- --match (-m) : 특정 모듈과의 매치

### 4) 타겟(target)
iptables는 패킷이 규칙과 일치할 때 동작을 취하는 타겟을 지원한다.

- ACCEPT : 패킷을 받아들인다.
- DROP : 패킷을 버린다(패킷이 전송된 적이 없던 것처럼).
- REJECT : 패킷을 버리고 이와 동시에 적절한 응답 패킷을 전송한다.
- LOG : 패킷을 syslog에 기록한다.
- RETURN : 호출 체인 내에서 패킷 처리를 계속한다.
REJECT는 서비스에 접속하려는 사용자의 액세스를 거부하고 connection refused라는 오류 메시지를 보여주는 반면 DROP은 말 그대로 telnet 사용자에게 어떠한 경고 메시지도 보여주지 않은 채 패킷을 드롭한다. 관리자의 재량껏 이러한 규칙을 사용할 수 있지만 사용자가 혼란스러워하며 계속해서 접속을 시도하는 것을 방지하려면 REJECT를 사용하는 것이 좋다.

### 5) 연결 추적(Connection Tracking)
iptables는 연결 추적(connection tracking)이라는 방법을 사용하여 내부 네트워크 상 서비스 연결 상태에 따라서 그 연결을 감시하고 제한할 수 있게 해준다. 연결 추적 방식은 연결 상태를 표에 저장하기 때문에, 다음과 같은 연결 상태에 따라서 시스템 관리자가 연결을 허용하거나 거부할 수 있다.

- NEW : 새로운 연결을 요청하는 패킷, 예, HTTP 요청
- ESTABLISHED : 기존 연결의 일부인 패킷
- RELATED : 기존 연결에 속하지만 새로운 연결을 요청하는 패킷, 예를 들면 접속 포트가 20인 수동 FTP의 경우 전송 포트는 사용되지 않은 1024 이상의 어느 포트라도 사용 가능하다.
- INVALID : 연결 추적표에서 어디 연결에도 속하지 않은 패킷
상태에 기반(stateful)한 iptables 연결 추적 기능은 어느 네트워크 프로토콜에서나 사용 가능하다. UDP와 같이 상태를 저장하지 않는 (stateless) 프로토콜에서도 사용할 수 있다.

### 6) 명령어(commond)
- -A (--append) : 새로운 규칙을 추가한다.
- -D (--delete) : 규칙을 삭제한다.
- -C (--check) : 패킷을 테스트한다.
- -R (--replace) : 새로운 규칙으로 교체한다.
- -I (--insert) : 새로운 규칙을 삽입한다.
- -L (--list) : 규칙을 출력한다.
- -F (--flush) : chain으로부터 규칙을 모두 삭제한다.
- -Z (--zero) : 모든 chain의 패킷과 바이트 카운터 값을 0으로 만든다.
- -N (--new) : 새로운 chain을 만든다.
- -X (--delete-chain) : chain을 삭제한다.
- -P (--policy) : 기본정책을 변경한다.
### 7) 기본 동작
패킷에 대한 동작은 위에서 부터 차례로 각 규칙에 대해 검사하고, 그 규칙과 일치하는 패킷에 대하여 타겟에 지정한 ACCEPT, DROP등을 수행한다.
규칙이 일치하고 작업이 수행되면, 그 패킷은 해당 규칙의 결과에 따리 처리하고 체인에서 추가 규칙을 무시한다.
패킷이 체인의 모든 규칙과 매치하지 않아 규칙의 바닥에 도달하면 정해진 기본정책(policy)이 수행된다.
기본 정책은 policy ACCEPT , policy DROP 으로 설정할 수 있다.
일반적으로 기본정책은 모든 패킷에 대해 DROP을 설정하고 특별히 지정된 포트와 IP주소등에 대해 ACCEPT를 수행하게 만든다.

### 8) iptables 출력
Iptables의 룰셋을 확인할때 아래와 같이 하면 보기 더 편리하다.

```bash
iptables -nL

  Chain INPUT (policy DROP)
  target     prot opt source               destination
  ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
  ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED
  ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:22
  ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:53
  ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0           udp dpt:53
  ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:80
  ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:443
  ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:3306

  Chain FORWARD (policy DROP)
  target     prot opt source               destination

  Chain OUTPUT (policy ACCEPT)
  target     prot opt source               destination
```
아래와 같이 각 룰셋의 적용순서까지 확인 가능한 방법도 있다.

```bash
iptables -nL --line-numbers

  Chain INPUT (policy DROP)
  num  target     prot opt source               destination
  1    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0
  2    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED
  3    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:22
  4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:53
  5    ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0           udp dpt:53
  6    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:80
  7    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:443
  8    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:3306

  Chain FORWARD (policy DROP)
  num  target     prot opt source               destination

  Chain OUTPUT (policy ACCEPT)
  num  target     prot opt source               destination
```

```bash
iptables -L -v

  Chain INPUT (policy DROP 1626 packets, 214K bytes)
   pkts bytes target     prot opt in     out     source               destination
      0     0 ACCEPT     all  --  lo     any     anywhere             anywhere
    944  194K ACCEPT     all  --  any    any     anywhere             anywhere            state RELATED,ESTABLISHED
      0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:ssh
      0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:domain
      4   245 ACCEPT     udp  --  any    any     anywhere             anywhere            udp dpt:domain
      6   304 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:http
      0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:https
      2    88 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:mysql

  Chain FORWARD (policy DROP 0 packets, 0 bytes)
   pkts bytes target     prot opt in     out     source               destination

  Chain OUTPUT (policy ACCEPT 179 packets, 22190 bytes)
   pkts bytes target     prot opt in     out     source               destination
```

## iptables 설정
아래는 CentOS 6.4 Minimal의 기본적인 iptables의 설정내용

```bash
iptables -L

  Chain INPUT (policy ACCEPT)
  target     prot opt source               destination
  ACCEPT     all  --  anywhere             anywhere            state RELATED,ESTABLISHED
  ACCEPT     icmp --  anywhere             anywhere
  ACCEPT     all  --  anywhere             anywhere
  ACCEPT     tcp  --  anywhere             anywhere            state NEW tcp dpt:ssh
  REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited

  Chain FORWARD (policy ACCEPT)
  target     prot opt source               destination
  REJECT     all  --  anywhere             anywhere            reject-with icmp-host-prohibited

  Chain OUTPUT (policy ACCEPT)
  target     prot opt source               destination
```
기본 정책이 모든 패킷에 대해 ACCEPT이며, SSH 서비스가 기본적으로 허용되어 있다. 이것을 과감히 날리고! 새로운 정책의 규칙을 작성할

> 기본 정책 수립에 있어 DROP으로 설정할 경우 원격에서 SSH를 접속해 사용중이라면 그 순간 서버에 접속할 수 없게 된다. 그러므로 일단 기본 정책을 ACCEPT로 설정해서 SSH 설정을 마친후 다시 기본 정책을 DROP으로 변경하도록 하자. 현재 iptables 작업을 콘솔(서버컴퓨터로)상으로 작업하고 있다면 문제 될것이 없다.


## 기본 설정

### 1. 기본 정책을 ACCEPT 로 변경

```bash
iptables -P INPUT ACCEPT
```
### 2. 체인에 정의된 모든 규칙을 삭제
```bash
iptables -F
```
### 3. 확인해보면 규칙이 모두 제거되어 있다.
```bash
iptables -L

  Chain INPUT (policy ACCEPT)
  target     prot opt source               destination

  Chain FORWARD (policy ACCEPT)
  target     prot opt source               destination

  Chain OUTPUT (policy ACCEPT)
  target     prot opt source               destination
```

### 4. INPUT 체인에 로컬호스트 인터페이스에 들어오는 모든 패킷을 허용 추가
```bash
iptables -A INPUT -i lo -j ACCEPT
```

### 5. INPUT 체인에 state 모듈과 매치되는 연결상태가 ESTABLISHED, RELATED인 패킷에 대해 허용 추가
```bash
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```
INPUT 체인에 접속에 속하는 패킷(응답 패킷을 가진것)과 기존의 접속 부분은 아니지만 연관성을 가진 패킷 (ICMP 에러나 ftp데이터 접속을 형성하는 패킷)을 허용하는 규칙이다.

### 6. INPUT 체인에 프로톨콜이 tcp이며 목적지포트가 22번인 패킷에 대해 허용 추가
```bash
iptables -A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
```
이로써 SSH 접속이 허용된다. telnet의 경우는 목적지 포트가 23번

### 7.이제 INPUT 체인에 대한 기본 정책을 버림(DROP)으로 변경
```bash
iptables -P INPUT DROP
```
### 8.FORWARD 체인에 대한 기본정책을 버림으로 변경
```bash
iptables -P FORWARD DROP
```
서버를 라우팅기기로 사용하지 않기에 모든 포워드에 대한 패킷을 DROP

### 9. OUTPUT 체인에 대한 기본정책을 허용으로 변경
```bash
iptables -P OUTPUT ACCEPT
```
### 10. 설정한 것들에 대한 확인
```bash
iptables -L -v

  Chain INPUT (policy DROP 108 packets, 12199 bytes)
   pkts bytes target     prot opt in     out     source               destination
      0     0 ACCEPT     all  --  lo     any     anywhere             anywhere
    273 25012 ACCEPT     all  --  any    any     anywhere             anywhere            state RELATED,ESTABLISHED
      0     0 ACCEPT     tcp  --  any    any     anywhere             anywhere            tcp dpt:ssh

  Chain FORWARD (policy DROP 0 packets, 0 bytes)
   pkts bytes target     prot opt in     out     source               destination

  Chain OUTPUT (policy ACCEPT 9 packets, 1612 bytes)
   pkts bytes target     prot opt in     out     source               destination
```
### 11. 설정한 것들 저장
```bash
service iptables save
```

