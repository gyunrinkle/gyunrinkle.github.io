---
layout: post
title: wsl2 ubuntu 설치 및 terminal 세팅
subtitle: 어렵다
categories: [etc]
tags: [wsl2, ubuntu, terminal]
comments: true
sitemap:
  changefreq: daily
---

```powershell
wsl --install Ubuntu
```

`username`이랑 `password` 새로 입력하고 로그인 하면 됨.
그리고 패키지 업데이트를 해주자.

```bash
sudo apt update -y && sudo apt upgrade -y && sudo apt auto-remove -y
```