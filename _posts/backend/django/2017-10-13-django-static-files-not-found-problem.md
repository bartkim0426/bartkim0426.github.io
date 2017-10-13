---
layout: post
title:  "Django static files not found - dev, deploy"
categories: django
tag: django static
---

# problem: django static files not found
장고를 development와 deploy level에서 동시에 개발을 진행하면 static files를 관리하기가 상당히 까다롭다. 
특히 deploy에서 이미 collectstatic이 되어 있는 상태에서, 직접 deploy 된 static server에서 front-end 개발자가 static file을 수정할 경우, 개발 환경에서 이 static을 일일히 적용시키기가 상당히 까다롭다. 


## In dev server


## Join dev + deploy server
