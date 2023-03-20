---
layout: post
title: Windows에 NVM 설치하기
subtitle: 이런 게 있는지 처음 알았다...
gh-repo: coreybutler/nvm-windows
gh-badge: [star, fork, follow]
categories: [web-dev]
tags: [node, nvm]
comments: true
sitemap:
  changefreq: daily
---

# 이전에 존재하던 node를 깔끔하게 삭제하자

revo uninstaller로 지워 주자

# winget으로 간단히 설치

```powershell
winget install --exact --id CoreyButler.NVMforWindows
```