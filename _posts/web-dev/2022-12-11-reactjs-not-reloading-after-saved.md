---
layout: post
title: WSL 2 ReactJS not reloading after saved
subtitle: package.json 설정 바꾸자
categories: [web-dev]
tags: [react, node]
comments: true
sitemap:
  changefreq: daily
---

[GitHub Discussion](https://github.com/facebook/create-react-app/issues/10253#issuecomment-1127340307)

```json
"scripts": {
    "start": "WATCHPACK_POLLING=true react-scripts start",
```