---
layout: post
title: devconatiner에서 gdbgui로 디버깅
subtitle: -r 옵션을 잘 이용하자
categories: [etc]
tags: [wsl2, devcontainer, gdb, gdbgui]
comments: true
sitemap:
  changefreq: daily
---
devcontainer에서 gdbgui를 이용해 디버깅하려는데 다른 옵션을 넣어도 다 안된다. 다음과 같이 하자.

```bash
gdbgui -r
```
