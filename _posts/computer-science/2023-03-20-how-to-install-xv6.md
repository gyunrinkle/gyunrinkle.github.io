---
layout: post
title: xv6 설치하기
subtitle: qemu를 설치하자...
gh-repo: mit-pdos/xv6-public
gh-badge: [star, fork, follow]
categories: [computer-science]
tags: [xv6, operating-system]
comments: true
sitemap:
  changefreq: daily
---

# xv6 개발 환경

- x86_64 lenovo laptop
- docker desktop (wsl2 backend) + vscode devconatiner (ubuntu-22.04 jammy)
- gcc (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0
- GNU Make 4.3
- 
```bash
sudo apt update -y && sudo apt upgrade -y && sudo apt auto-remove -y
sudo apt install qemu-kvm -y
git clone https://github.com/mit-pdos/xv6-public.git
cd xv6-public
make
make qemu-nox
```
