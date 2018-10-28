---
layout: post
title:  "MAC: ERD 그릴 수 있는 무료 툴 소개"
categories: mac
---


이번에 새롭게 프로젝트를 진행하면서 db를 구성할 때 ERD를 그려보기로 했다.

> ERD란? Entity–relationship model의 약자로 존재하고 있는 관계들을 그림으로 그려내어 관계를 도출해 내는 것이다.

처음에는 종이에 손으로 그려보려다가 추후에 수정사항을 변경하거나 유지보수를 하기가 어려울 것 같아 ERD를 그릴 수 있는 툴을 찾아보았다.

역시 비싼 ERD 툴틀은 수백만원이 훌쩍 넘는데, 그런 기능과 가격의 툴을 찾는게 아니므로 여기서 소개하지 않고 무료 툴 위주로 소개하겠다.


### [mysql workbench](https://dev.mysql.com/downloads/workbench/)

타 툴들과는 다르게 오라클 산하 MySQL에서 제공하는 툴이기 때문에 매우 기능이 다양하고 깔끔한 ui로 구성되어 있다. 뿐만 아니라 mac, window, linux까지 폭넓은 지원 범위를 자랑한다.

다만 다운받기 위해서는 `Oracle` 통합 회원가입을 필수로 해야 한다. 


`Mysql workbench`는 ERD를 그릴 수 있는 기능 뿐 아니라 

- 데이터베이스 디자인&모델링
- SQL development
- DB administration
- DB migration

등의 기능을 제공한다.

![mysql workbench](https://i.imgur.com/5d5DpUz.png){:class="img-responsive"}

실제로 필드를 입력하고 관계를 형성하는데 매우 좋지만, 그냥 기본적인 형태의 ERD를 그리고 싶은 사람이라면 다운로드/설치 및 사용이 조금 번거로울 수 있다.


### [Ondras](http://ondras.zarovi.cz/sql/demo/)

Ondras는 정말 ERD의 기본적인 기능을 제공하는 웹 기반 서비스이다.

기본적으로 테이블과 필드, 관계 등을 생성하여 깔끔한 ui로 그릴 수 있다. 웹으로 구현되기 때문에 언제 어디서든 간편하게 사용할 수 있다는 장점도 있다.

다만 단점으로는 1:1관계만을 지원하고, save/load 기능이 있지만 제대로 사용하기가 힘들어서 일회성으로 사용해야 하는 경우가 생긴다.

db모델링을 하기 전에 간단하게 테이블들을 한눈에 보고 싶을 때 사용하면 좋을 것 같다.

![ondras screenshot](https://i.imgur.com/6ArDl5x.png){:class="img-responsive"}



### [Aquery tool](http://aquerytool.com/)

이 툴은 특이하게도 한 개발자분이 직접 만들어서 한 커뮤니티에 올리신 웹사이트이다. (https://okky.kr/article/312693)

[여기](http://aquerytool.com/?demo=y)에서 직접 올린 데모 데이터를 확인해 볼 수 있다.


![Aquery screenshot](https://i.imgur.com/0Nqrmz4.png){:class="img-responsive"}


웹 기반 1인 개발 프로그램임에도 상당히 기능들이 잘 구현되어 있고 sql 테스트 데이터도 제공해준다. save/load 등의 기능도 잘 되어있어서 무료로 사용하기 참 좋은 툴이 아닐까 싶다.



