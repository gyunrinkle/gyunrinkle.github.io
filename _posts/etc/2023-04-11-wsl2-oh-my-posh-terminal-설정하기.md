---
layout: post
title: oh my posh 터미널 설정하기
subtitle: Each post also has a subtitle
categories: [etc]
tags: [wsl, bash, ohmyposh]
comments: true
sitemap:
  changefreq: daily
---
먼저 다음 패키지들을 설치한다.
```bash
sudo apt-get install build-essential procps curl file git -y
```
먼저 wsl에서 `brew`를 설치한다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

그 다음 다음 명령어를 복사 붙여 넣기한다.

```bash
test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)"
test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
test -r ~/.bash_profile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.bash_profile
echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.profile
```

그 다음 `brew`가 잘 설치됐는지 확인한다.

```bash
brew install hello
```

`brew install`이 너무 느릴 수도 있는데, 그럴 때는 `powershell`를 Admin 권한으로 켜서 DNS Cache를 Flush한다.

```powershell
ipconfig /flushdns
```

암튼 아래 명령어로 `oh-my-posh`를 설치한다.

```bash
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```