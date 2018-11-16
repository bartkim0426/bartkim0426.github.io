---
layout: post
title:  "VIM: case sensitive / insensitive when searching"
categories: vim
---


## Change set

```
# to search without case sensitive
:set ignorecase
# to search with case sensitive
:set noignorecase
```

## Tmp search

```
# to search without case sensitive
/\cword

# to search with case sensitive
/\CWORD
```
