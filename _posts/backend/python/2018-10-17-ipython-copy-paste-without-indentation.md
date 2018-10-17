---
layout: post
title:  "ipython: copy and paste without indentation"
categories: python
---


When using ipython, sometimes it's very uncomfortable for copy & paste some code. Ipython use auto indentation, so it goes wrong with indent.

I want to figure out how to solve this issue - and I found the solution.

IPython has `--no-autoindent` options. For default, ipython use `--autoindent` but you can use `--no-autoindent` option to turn off indentation.


```
ipython --no-autoindent

Python 3.6.0 (default, Mar 17 2017, 22:33:12)
Type 'copyright', 'credits' or 'license' for more information
IPython 6.2.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: def test_indent():
   ...:     print('testing --no-autoindent')
   ...:
```
