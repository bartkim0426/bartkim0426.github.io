---
layout: post
title:  "AWS : copy file from aws ec2 to local"
categories: aws
---


It's easy to copy file from aws to local. Just use `scp -i <path_to_pem> <username>@<ip>:<path_to_target_file> <path_to_download>`

```bash
scp -i ~/.ssh/path/to/pem ubuntu@52.79.xxx.x:/home/ubuntu/path/to/file ~/path/to/download
```
