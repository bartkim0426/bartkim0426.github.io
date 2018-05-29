---
layout: post
title:  "Find files in mac terminal"
categories: terminal
---


## 1. Using `grep`

```bash
grep -r 'text_here' path_here
```

use `man grep` for more information


## 2. Using spotlight

```bash
mdfind 'text here'
```

Also can use `man mdfind`


## 3. Use Ack

```bash
brew install ack
ack 'text_here'
```
