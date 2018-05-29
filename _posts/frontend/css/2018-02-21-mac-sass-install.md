---
layout: post 
title:  "Mac sass install and usage"
categories: css
---

### Install

```bash
gem install sass
# or sudo gem install sass
```

When installed, check version

```bash
sass -v
```

### Compile usage

```bash
# compile one file
sass input.scss output.css

# watching (automatically compile)
sass --watch input.scss:output.css

# set style
sass --watch --style compressed input.scss output.css
```
