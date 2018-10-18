---
layout: post
title:  "MAC: solving mojave xcode problem"
categories: mac
---


After upgrade mac software from `High Sierra` to `Mojave`, I found some bugs in my terminal.

When I tried to use `git` command, the error message occurs like below

```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

After finding few posts and questions, I found the solution to

```
xcode-select --install
```

This will download and/or install xcode tools.

You may need to set path to Xcode by next command

```
xcode-select --switch /Applications/Xcode.app
xcode-select --switch /Library/Developer/CommandLineTools
```
