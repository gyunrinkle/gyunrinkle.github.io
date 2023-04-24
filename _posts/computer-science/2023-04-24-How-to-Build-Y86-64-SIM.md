---
layout: post
title: Y86-84 Simulator Build 방법
subtitle: Makefile을 수정하자
categories: [computer-science]
tags: [system-programming, computer-architecture]
comments: true
sitemap:
  changefreq: daily
---


```bash
wget http://csapp.cs.cmu.edu/3e/sim.tar
tar xf sim.tar
```

GUI 버전으로 Build하기 위해서는 `tk`, `tk-dev`, `tcl`, `tcl-dev` package를 미리 설치해 줘야 한다.

```bash
sudo apt update -y && sudo apt install -y tcl tcl-dev tk tk-dev
```