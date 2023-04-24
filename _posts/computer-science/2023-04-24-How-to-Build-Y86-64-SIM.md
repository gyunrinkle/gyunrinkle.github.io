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

# GUI PKG 설치 및 `Makefile` 변경

GUI 버전으로 Build하기 위해서는 `tk`, `tk-dev`, `tcl`, `tcl-dev` package를 미리 설치해 줘야 한다.

```bash
sudo apt update && sudo apt install tcl tcl-dev tk tk-dev -y
```

`make`로 빌드하기 전에 `sim/Makefile`을 잠시 살펴 보자.

```c
# Comment this out if you don't have Tcl/Tk on your system

GUIMODE=-DHAS_GUI

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

```c
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

```c
TKINC=-isystem /usr/include/tcl8.6
```

# `make`로 Build 시 오류 잡기

```bash
make clean
make
```

`make`로 build를 하면 오류가 엄청 뿜어져 나온다.

```bash
(cd misc; make all)
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/misc'
gcc -Wall -O1 -g -c yis.c
gcc -Wall -O1 -g -c isa.c
gcc -Wall -O1 -g yis.o isa.o -o yis
gcc -Wall -O1 -g -c yas.c
flex yas-grammar.lex
make[1]: flex: No such file or directory
make[1]: *** [Makefile:22: yas-grammar.c] Error 127
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/misc'
make: *** [Makefile:26: all] Error 2
```

음... `flex` 라는 명령어가 없어서 에러가 난 거 같다.

```bash
sudo apt install flex -y
```

설치를 해주고, 다시 build하자.

```bash
make clean
make
```

```bash
(cd misc; make all)
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/misc'
gcc -Wall -O1 -g -c yis.c
gcc -Wall -O1 -g -c isa.c
gcc -Wall -O1 -g yis.o isa.o -o yis
gcc -Wall -O1 -g -c yas.c
flex yas-grammar.lex
mv lex.yy.c yas-grammar.c
gcc -O1 -c yas-grammar.c
gcc -Wall -O1 -g yas-grammar.o yas.o isa.o -lfl -o yas
/usr/bin/ld: yas.o:/workspaces/comedu-computer-architecture/sim/misc/yas.h:13: multiple definition of `lineno'; yas-grammar.o:(.bss+0x0): first defined here
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile:32: yas] Error 1
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/misc'
make: *** [Makefile:26: all] Error 2
```

다시 오류가 난다. 오류를 보니 `lineno`랑 문제가 있는 것 같다.

<https://stackoverflow.com/questions/63152352/fail-to-compile-the-y86-simulatur-csapp>를 참고해서 trouble shooting을 했다. (~~shout to 컴구 교수님~~)

gcc-10부터 default gcc compile flag가 `-fcommon`에서 `-fno-common`으로 바뀌었기 때문이다.
그래서`sim/misc/Makefile`을 다음과 같이 변경한다.

```c
CFLAGS=-Wall -O1 -g -fcommon
LCFLAGS=-O1 -fcommon
```