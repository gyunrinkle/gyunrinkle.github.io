---
layout: post
title: docker desktop를 쓰면 Vmmem이라는 process가 메모리를 너무 많이 잡아 먹는다.
subtitle: 일단 docker desktop을 삭제했다.
categories: [etc]
tags: [docker, wsl2]
comments: true
sitemap:
  changefreq: daily
---

ssd를 교체했더니, docker를 쓸 때마다 memory leak이 엄청 발생한다.
docker desktop을 일단 삭제, wsl2를 삭제하고 다시 깔자.
그러니 해결됐다.
그러니 평소에 모든 작업을 웬만하면 docker devcontainer에서 진행하는 게 좋을 거 같다. 그러면 이런 경우에도 여차하면 지우고, 다시 깔아도 큰 타격이 없으니깐