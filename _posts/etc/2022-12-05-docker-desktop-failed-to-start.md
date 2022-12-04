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

Windows Terminal에서 winget으로Docker Desktop 업데이트 하다가 업데이트 속도가 너무 느려서, 업데이트 중간에 강제종료했는데, 그 이후로 이런 화면이 뜨면서 갑자기 Docker가 안된다. (~~개킹받아ㅠ~~)

# 다시 깔자...

별 짓을 다 해봤는데 그냥 Windows Docker Desktop에는 자체적으로 오류가 꽤 있는 거 같다. revo uninstaller로 삭제하고, <https://www.docker.com/products/docker-desktop/>으로 가서 다시 깔았다....

# 그래도 안된다

개발자들의 최후의 비기 들어갔다. 구글링을 엄청했다. 그랬더니 stackoverflow에 나랑 비슷한 증상을 호소한 사람이 있었다. 그래서 그 분의 해결책을 참고했다.

>My Docker Desktop failed to start after I forced it to exit while updating (it stuck during the update that's why I had to do it). No solution on the Internet helped me until I ran into this Powershell command:

```
wsl -l -v
```

> It listed the following:

```
* Ubuntu-18.04           Stopped         2
  docker-desktop         Uninstalling    2
  docker-desktop-data    Stopped         2
```

> It kept saying "Uninstalling" even after rebooting the whole system.

> What I did was:

```
wsl -t docker-desktop
```

> It terminated docker-desktop and made the problem gone

> [출처](https://stackoverflow.com/questions/67406780/not-able-to-start-docker-desktop-in-windows/73733654#73733654)
