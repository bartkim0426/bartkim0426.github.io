---
layout: post
title:  "Node.js: supervisor install and usage"
categories: nodejs
---

When you use node.js during devloping, you should restart every time when you edit code. 

For solving this issue, you can use `supervisor` for watch project changing.


[`Supervisor`](https://www.npmjs.com/package/supervisor) is famouse npm package that download over 817,724 time (data from [`npm-stat.com`](https://www.npmjs.com/package/supervisor))


### Install

Install by `npm`

```bash
npm install supervisor -g
```

### Usage

Usage is simple. Instead of `node app.js` command, just use `supervisor app.js`. Then it automatically detect changes from code and restart server.

```bash
supervisor app.js
```

> for more information, check [`supervisor documentation`](http://supervisord.org/index.html).
