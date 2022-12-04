---
layout: post
title: Windows "Docker Desktop failed to start" 오류
subtitle: 삭제했다가 다시 깔자...
categories: [etc]
tags: [docker]
comments: true
sitemap:
  changefreq: daily
---

# Docker Desktop이 갑자기 업데이트 되고 나서 잘 안된다.

![오류 화면](/assets/img/2022-12-05-docker-desktop-failed-to-start/오류.png)

Docker Desktop 업데이트 후, 이런 화면이 뜨면서 갑자기 Docker가 안된다. (~~개킹받아ㅠ~~)

# 다시 깔자...

별 짓을 다 해봤는데 그냥 Windows Docker Desktop에는 자체적으로 오류가 꽤 있는 거 같다. revo uninstaller로 삭제하고, winget으로 다시 깔았다....
```powershell
winget install -e --id Docker.DockerDesktop
```