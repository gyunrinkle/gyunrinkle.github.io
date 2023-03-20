---
layout: post
title: Windows에 NVM 설치하기
subtitle: 이런 게 있는지 처음 알았다...
gh-repo: coreybutler/nvm-windows
gh-badge: [star, fork, follow]
categories: [web-dev]
tags: [node, nvm]
comments: true
sitemap:
  changefreq: daily
---

# 이전에 존재하던 node를 깔끔하게 삭제하자

revo uninstaller로 지워 주자

# winget으로 간단히 설치

```powershell
winget install --exact --id CoreyButler.NVMforWindows
```

# 재부팅 해야 겠지?

Windows에서 뭔가를 설치하면 재부팅하는 건 국룰!

# `nvm` 실행 잘 된다

```powershell
nvm
```

```powershell
Running version 1.1.10.

Usage:

  nvm arch                     : Show if node is running in 32 or 64 bit mode.
  nvm current                  : Display active version.
  nvm install <version> [arch] : The version can be a specific version, "latest" for the latest current version, or "lts" for the
                                 most recent LTS version. Optionally specify whether to install the 32 or 64 bit version (defaults
                                 to system arch). Set [arch] to "all" to install 32 AND 64 bit versions.
                                 Add --insecure to the end of this command to bypass SSL validation of the remote download server.
  nvm list [available]         : List the node.js installations. Type "available" at the end to see what can be installed. Aliased as ls.
  nvm on                       : Enable node.js version management.
  nvm off                      : Disable node.js version management.
  nvm proxy [url]              : Set a proxy to use for downloads. Leave [url] blank to see the current proxy.
                                 Set [url] to "none" to remove the proxy.
  nvm node_mirror [url]        : Set the node mirror. Defaults to https://nodejs.org/dist/. Leave [url] blank to use default url.
  nvm npm_mirror [url]         : Set the npm mirror. Defaults to https://github.com/npm/cli/archive/. Leave [url] blank to default url.
  nvm uninstall <version>      : The version must be a specific version.
  nvm use [version] [arch]     : Switch to use the specified version. Optionally use "latest", "lts", or "newest".
                                 "newest" is the latest installed version. Optionally specify 32/64bit architecture.
                                 nvm use <arch> will continue using the selected version, but switch to 32/64 bit mode.
  nvm root [path]              : Set the directory where nvm should store different versions of node.js.
                                 If <path> is not set, the current root will be displayed.
  nvm [--]version              : Displays the current running version of nvm for Windows. Aliased as v.

```