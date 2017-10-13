---
layout: post
title:  "Kill all gunicorn process"
categories: server
tag: gunicorn
---

## Get gunicorn pid

`pstree -ap | grep gunicorn`



## Kill gunicorn pid
`kill pid`


## Run gunicorn again
`gunicorn --bind unix:/tmp/bestlab.sock --workers 3 wsgi:application`
