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

```bash
make clean
make
```
드디어 빌드가 되나?! 싶었는데 또 오류다.

```bash
(cd misc; make all)
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/misc'
gcc -Wall -O1 -g -fcommon -c yis.c
gcc -Wall -O1 -g -fcommon -c isa.c
gcc -Wall -O1 -g -fcommon yis.o isa.o -o yis
gcc -Wall -O1 -g -fcommon -c yas.c
flex yas-grammar.lex
mv lex.yy.c yas-grammar.c
gcc -O1 -fcommon -c yas-grammar.c
gcc -Wall -O1 -g -fcommon yas-grammar.o yas.o isa.o -lfl -o yas
bison -d hcl.y
make[1]: bison: No such file or directory
make[1]: *** [Makefile:53: hcl.tab.c] Error 127
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/misc'
make: *** [Makefile:26: all] Error 2
```

이번에는 `bison`이 없는 거 같다. 설치를 해 주자.

```bash
sudo apt install bison -y
```

```bash
make clean
make
```

아 이제 build 되나? ㅋㅋ

```bash
(cd misc; make all)
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/misc'
gcc -Wall -O1 -g -fcommon -c yis.c
gcc -Wall -O1 -g -fcommon -c isa.c
gcc -Wall -O1 -g -fcommon yis.o isa.o -o yis
gcc -Wall -O1 -g -fcommon -c yas.c
flex yas-grammar.lex
mv lex.yy.c yas-grammar.c
gcc -O1 -fcommon -c yas-grammar.c
gcc -Wall -O1 -g -fcommon yas-grammar.o yas.o isa.o -lfl -o yas
bison -d hcl.y
flex hcl.lex
gcc -O1 -fcommon node.c lex.yy.c hcl.tab.c outgen.c -o hcl2c
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/misc'
(cd pipe; make all GUIMODE=-DHAS_GUI TKLIBS="-L/usr/lib/x86_64-linux-gnu/ -ltk -ltcl" TKINC="-isystem /usr/include/tcl8.6")
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/pipe'
# Building the pipe-std.hcl version of PIPE
../misc/hcl2c -n pipe-std.hcl < pipe-std.hcl > pipe-std.c
gcc -Wall -O2 -isystem /usr/include/tcl8.6 -I../misc -DHAS_GUI -o psim psim.c pipe-std.c \
        ../misc/isa.c -L/usr/lib/x86_64-linux-gnu/ -ltk -ltcl -lm
psim.c: In function ‘simResetCmd’:
psim.c:853:15: error: ‘Tcl_Interp’ has no member named ‘result’
  853 |         interp->result = "No arguments allowed";
      |               ^~
psim.c:861:11: error: ‘Tcl_Interp’ has no member named ‘result’
  861 |     interp->result = stat_name(STAT_AOK);
      |           ^~
psim.c: In function ‘simLoadCodeCmd’:
psim.c:872:15: error: ‘Tcl_Interp’ has no member named ‘result’
  872 |         interp->result = "One argument required";
      |               ^~
psim.c:878:15: error: ‘Tcl_Interp’ has no member named ‘result’
  878 |         interp->result = tcl_msg;
      |               ^~
psim.c:885:11: error: ‘Tcl_Interp’ has no member named ‘result’
  885 |     interp->result = tcl_msg;
      |           ^~
psim.c: In function ‘simLoadDataCmd’:
psim.c:895:11: error: ‘Tcl_Interp’ has no member named ‘result’
  895 |     interp->result = "Not implemented";
      |           ^~
psim.c:901:15: error: ‘Tcl_Interp’ has no member named ‘result’
  901 |         interp->result = "One argument required";
      |               ^~
psim.c:907:15: error: ‘Tcl_Interp’ has no member named ‘result’
  907 |         interp->result = tcl_msg;
      |               ^~
psim.c:911:11: error: ‘Tcl_Interp’ has no member named ‘result’
  911 |     interp->result = tcl_msg;
      |           ^~
psim.c: In function ‘simRunCmd’:
psim.c:925:15: error: ‘Tcl_Interp’ has no member named ‘result’
  925 |         interp->result = "At most one argument allowed";
      |               ^~
psim.c:932:15: error: ‘Tcl_Interp’ has no member named ‘result’
  932 |         interp->result = tcl_msg;
      |               ^~
psim.c:936:11: error: ‘Tcl_Interp’ has no member named ‘result’
  936 |     interp->result = stat_name(status);
      |           ^~
psim.c: In function ‘simModeCmd’:
psim.c:945:15: error: ‘Tcl_Interp’ has no member named ‘result’
  945 |         interp->result = "One argument required";
      |               ^~
psim.c:948:11: error: ‘Tcl_Interp’ has no member named ‘result’
  948 |     interp->result = argv[1];
      |           ^~
psim.c:957:15: error: ‘Tcl_Interp’ has no member named ‘result’
  957 |         interp->result = tcl_msg;
      |               ^~
psim.c: In function ‘signal_register_update’:
psim.c:994:63: error: ‘Tcl_Interp’ has no member named ‘result’
  994 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘create_memory_display’:
psim.c:1005:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1005 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c:1020:67: error: ‘Tcl_Interp’ has no member named ‘result’
 1020 |             fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                                   ^~
psim.c: In function ‘set_memory’:
psim.c:1055:67: error: ‘Tcl_Interp’ has no member named ‘result’
 1055 |             fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                                   ^~
psim.c: In function ‘show_cc’:
psim.c:1069:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1069 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘show_stat’:
psim.c:1081:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1081 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘show_cpi’:
psim.c:1096:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1096 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘signal_sources’:
psim.c:1110:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1110 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘signal_register_clear’:
psim.c:1120:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1120 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘report_line’:
psim.c:1134:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1134 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘report_pc’:
psim.c:1190:63: error: ‘Tcl_Interp’ has no member named ‘result’
 1190 |         fprintf(stderr, "Error Message was '%s'\n", sim_interp->result);
      |                                                               ^~
psim.c: In function ‘report_state’:
psim.c:1204:65: error: ‘Tcl_Interp’ has no member named ‘result’
 1204 |         fprintf(stderr, "\tError Message was '%s'\n", sim_interp->result);
      |                                                                 ^~
make[1]: *** [Makefile:44: psim] Error 1
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/pipe'
make: *** [Makefile:27: all] Error 2
```

😢😢 또 오류가 난다.

<https://onestepcode.com/install-csapp-y86-64-processor-simulator/>이 웹사이트의 trouble shooting 방법을 참고 했다.

이유는 source code 중 일부가 deprecated 된 instruction을 쓰고 있어서이다. `sim/seq/ssim.c` 과 `sim/pipe/psim.c`의 코드 일부를 다음과 같이 수정한다.

> All of the changes involve the substitution of the `interp->result = "some string";` lines with `Tcl_SetResult(interp, "some string", TCL_STATIC);`. Similarly, the `fprintf(stderr, "some string", sim_interp->result);` instructions need to be substituted with `fprintf(stderr, "some string", Tcl_GetStringResult(sim_interp));`. These lines need to be changed in both `psim.c` and `ssim.c`.

```bash
make clean
make
```

이제 되나..?!

```bash
(cd misc; make all)
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/misc'
gcc -Wall -O1 -g -fcommon -c yis.c
gcc -Wall -O1 -g -fcommon -c isa.c
gcc -Wall -O1 -g -fcommon yis.o isa.o -o yis
gcc -Wall -O1 -g -fcommon -c yas.c
flex yas-grammar.lex
mv lex.yy.c yas-grammar.c
gcc -O1 -fcommon -c yas-grammar.c
gcc -Wall -O1 -g -fcommon yas-grammar.o yas.o isa.o -lfl -o yas
bison -d hcl.y
flex hcl.lex
gcc -O1 -fcommon node.c lex.yy.c hcl.tab.c outgen.c -o hcl2c
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/misc'
(cd pipe; make all GUIMODE=-DHAS_GUI TKLIBS="-L/usr/lib/x86_64-linux-gnu/ -ltk -ltcl" TKINC="-isystem /usr/include/tcl8.6")
make[1]: Entering directory '/workspaces/comedu-computer-architecture/sim/pipe'
# Building the pipe-std.hcl version of PIPE
../misc/hcl2c -n pipe-std.hcl < pipe-std.hcl > pipe-std.c
gcc -Wall -O2 -isystem /usr/include/tcl8.6 -I../misc -DHAS_GUI -o psim psim.c pipe-std.c \
        ../misc/isa.c -L/usr/lib/x86_64-linux-gnu/ -ltk -ltcl -lm
/usr/bin/ld: /tmp/ccnza7vP.o:(.bss+0x0): multiple definition of `mem_wb_state'; /tmp/ccja0V8S.o:(.bss+0x120): first defined here
/usr/bin/ld: /tmp/ccnza7vP.o:(.bss+0x8): multiple definition of `ex_mem_state'; /tmp/ccja0V8S.o:(.bss+0x128): first defined here
/usr/bin/ld: /tmp/ccnza7vP.o:(.bss+0x10): multiple definition of `id_ex_state'; /tmp/ccja0V8S.o:(.bss+0x130): first defined here
/usr/bin/ld: /tmp/ccnza7vP.o:(.bss+0x18): multiple definition of `if_id_state'; /tmp/ccja0V8S.o:(.bss+0x138): first defined here
/usr/bin/ld: /tmp/ccnza7vP.o:(.bss+0x20): multiple definition of `pc_state'; /tmp/ccja0V8S.o:(.bss+0x140): first defined here
/usr/bin/ld: /tmp/ccja0V8S.o:(.data.rel+0x0): undefined reference to `matherr'
collect2: error: ld returned 1 exit status
make[1]: *** [Makefile:44: psim] Error 1
make[1]: Leaving directory '/workspaces/comedu-computer-architecture/sim/pipe'
make: *** [Makefile:27: all] Error 2
```

아 ㅋㅋ;;

이번에는 
```bash
/usr/bin/ld: /tmp/ccja0V8S.o:(.data.rel+0x0): undefined reference to `matherr'
```

이 error다.

이 에러는 `matherr` symbol이 더 이상 Glibc의 part가 아니여서 생기는 오류다. `psim.c`에서 한 줄만 comment하면 해결된다.

