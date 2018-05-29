---
layout: post
title:  "Vim: searching include slash"
categories: vim
---

In vim, you can easily search words by using `/` command.

But if you enter slash in search it doesn't match because `/` has special function in vim search. 

So if you want to search with slash, you simply add `\` before slash.

If you want to search the url including slash `example.com/user/`, use like this 

```vim
/example.com\/user\/
```
