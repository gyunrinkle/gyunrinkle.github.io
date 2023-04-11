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


> You may want to consider just running the gdbgui in the container, and interacting with it via a browser on the host. In order to do that I've found that I need the -r option when invoking gdbgui. Apparently this affects the gdbgui binding. My understanding is that without the -r option, gdbgui can only listen to http from inside the container, but with it, we can connect via docker networking. So from your browser on the host try connecting to localhost:5000, or whatever address gdbgui gives when it starts.


레퍼런스: <https://github.com/cs01/gdbgui/issues/247>