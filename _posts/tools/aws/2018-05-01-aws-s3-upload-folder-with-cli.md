---
layout: post
title:  "AWS : upload folder to s3 with cli"
categories: aws
---


Can simply upload directory with `aws` command.

```bash
aws s3 cp SOURCE_DIR s3://DEST_BUCKET/ --recursive
```

Or Sync by

```bash
aws s3 sync SOURCE_DIR s3://DEST_BUCKET/
```
