---
layout: post
title: Python 버전 관리하기
subtitle: 헷갈려서 기록
categories: [etc]
tags: [python]
comments: true
sitemap:
  changefreq: daily
---

### ***Windows OS 기준으로 설명합니다.***
---
# Python Runtime을 여러 개 설치
---
Docker를 쓰는 게 아닌 이상, 일단 Python Runtime을 여러 개 설치해야 한다.
<python.org/downloads>에서 혹은 winget으로 본인이 원하는 Runtime을 여러 개 설치한다.
필자는 3.11.1을 설치했었는데, stable-diffusion-webui라는 프로젝트가 3.10.6 버전 런타임을 요구해서 3.10.6을 공식 웹사이트에서 Windows용 Installer를 받아서 설치했다.
설치할 때 `Add to PATH` 는 꼭 체크하자. 그래야 사용자 환경 변수에 Python Runtime이 추가돼서 Powershell에서 바로 쓸 수 있다.

# 설치 결과
---
```powershell
python --version
Python 3.11.1
```
(필자는 3.11.1 버전을 먼저 설치해 놓았었다.)
엥? 왜 3.10.6을 설치하고, 해당 버전의 런타임을 환경 변수에 추가까지 해놓았는데 왜 이전과 동일하게 python 버전이 3.11.1로 나오지?

# 왜냐면 사용자 환경 변수에서 우선 순위가 다르기 때문...!
---
그렇다면 3.10.6 버전의 런타임이 설치 됐는지는 어떻게 확일할까?
```powershell
py -3.10 --version
Python 3.10.6
```
다음과 같이 `py -[런타임 버전]` 을 명시해 주면 된다. (3.10.6 이라고 하면 안된다.. 그건 왜 그런지 모르겠다. 꼭 3.10이라고 명시해 주어야 한다.)

# `venv` 모듈을 이용해 가상 환경을 구성
---
```powershell
py -3.10 -m venv venv
```