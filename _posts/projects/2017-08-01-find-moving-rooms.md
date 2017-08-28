---
layout: post
title:  "직방, 다방, 네이버 부동산, 피터팬을 모아서 내 방을 찾자!"
categories: project
tags: project crawling python django
---

## Moving Project

crawling rooms for specific condition 

- zigbang
- dabang
- piterpan


### zigbang

- zigbang api에서 보증금 1억, 월세 30, 홍대 근처 지역의 room id list 구하기

- 얻은 room-id를 가지고 detail api를 통해 각각 방의 detail json 정보 구하기

- json 정보를 JsonField model에 저장(model에 없는 아이디일 경우에만)
=> formview를 사용해서? 한 뷰에 두가지를 같이 넣을수는 없을듯
=> 따로 view를 사용하지 않고 cron? celery를 사용해서 돌리자!

- 저장된 정보들을 listing하는 리스트 페이지 만들기(listview)
