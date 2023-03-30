---
layout: post
title: tsc로 ts를 js로 transpiling 안해도 된다.
subtitle: 웹개발의 세계는 어지럽다.
categories: [web-dev]
tags: [node, typescript, express]
comments: true
sitemap:
  changefreq: daily
---

`npm install ts-node -D`로 `ts-node` 설치하고 다음 명령어로 `node`대신 `ts-node` runtime을 사용할 수 있다.
```powershell
npx ts-node app.ts
```