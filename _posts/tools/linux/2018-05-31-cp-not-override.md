---
layout: post
title:  "linux: copy file not override"
categories: linux
---


### Using cp -n

```
man  cp

-n, --no-clobber
              do not overwrite an existing file (overrides a previous -i option)


cp -n test.txt test2.txt
```


### Using `rsync`


```
# update files if differ
rsync -a -v <src_dir> <dst_dir>

# ignore existing file
rsync -a -v --ignore-existing <src_dir> <dst_dir>
```
