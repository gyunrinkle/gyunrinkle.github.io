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

실습 환경: 
```bash
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.2 LTS
Release:        22.04
Codename:       jammy
```

```bash
wget http://csapp.cs.cmu.edu/3e/sim.tar
tar xf sim.tar
```

GUI 버전으로 Build하기 위해서는 `tk`, `tk-dev`, `tcl`, `tcl-dev` package를 미리 설치해 줘야 한다.

```bash
sudo apt update && sudo apt install tcl tcl-dev tk tk-dev -y
```

`make`로 빌드하기 전에 `sim/Makefile`을 잠시 살펴 보자.

```
# Comment this out if you don't have Tcl/Tk on your system

#GUIMODE=-DHAS_GUI

# Modify the following line so that gcc can find the libtcl.so and
# libtk.so libraries on your system. You may need to use the -L option
# to tell gcc which directory to look in. Comment this out if you
# don't have Tcl/Tk.

TKLIBS=-L/usr/lib -ltk -ltcl

# Modify the following line so that gcc can find the tcl.h and tk.h
# header files on your system. Comment this out if you don't have
# Tcl/Tk.

TKINC=-isystem /usr/include/tcl8.5
```

`GUIMODE`로 build 하려면 일단 3번 째 줄을 **uncomment**해 줘야 한다. 그리고 gcc가 build를 할 때 `libtcl.so`과 `libtk.so` library를 알맞은 directory에 찾을 수 있도록 path를 정확히 설정해줘야 한다. `tcl.h`과 `tk.h` 또한 그렇다.

```bash
sudo find / -iname libtk.so
```

그러면 다음과 결과 나올 것이다.

```bash
/usr/lib/x86_64-linux-gnu/libtk.so
```

그러면 `Makefile`을 다음과 같이 바꾼다.

```
TKLIBS=-L/usr/lib/x86_64-linux-gnu/ -ltk -ltcl
```

```bash
sudo find / -iname tcl.h
```

그러면 다음과 같은 검색 결과가 나온다.

```bash
/usr/include/tcl8.6/tcl-private/generic/tcl.h
/usr/include/tcl8.6/tcl.h
```

`Makefile`을 다음과 같이 변경한다.

```
TKINC=-isystem /usr/include/tcl8.6
```