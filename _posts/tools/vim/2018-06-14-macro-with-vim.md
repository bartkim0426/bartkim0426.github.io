---
layout: post
title:  "Vim : macro with vim - replay, multiple times"
categories: vim
---


### Macro basic

```
# start recording macro
q{key}

# end recording macro
q

# play macro
@{key}
```

### replay macro several times 

```
# Select all visual block and do macro
VG:normal @{key}

# Just repeat macro 15 times
15@{key}

# command line
:%normal @{key}
```


