---
layout: post
title:  "Adjust cursor speed in mac (os x)"
categories: mac
---

I usally use terminal in mac. Mac default cursor speed is little bit slow so I changed cursor speed faster.

There's two way to adjust cursor speed in mac.

### 1. Change in mac settings

Change `repeat delay time` in mac system preference.

```
System Preference => Keyboard => Repeat Delay Time
```

### 2. Change in termianl
Open terminal app and enter this command

```bash
defaults write NSGlobalDomain KeyRepeat -int 0
```
