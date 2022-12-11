---
layout: post
title: Spring boot Windows에서 빌드
subtitle: Windows Terminal에서 빌드 
categories: [web-dev]
tags: [spring, spring-boot]
comments: true
sitemap:
  changefreq: daily
---

```powershell
.\gradlew.bat build
cd build\libs
java -jar hello-spring-0.0.1-SNAPSHOT.jar
```

# 빌드 폴더 삭제
```powershell
gradlew clean
```