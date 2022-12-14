---
layout: post
title: Spring Boot lombok 사용법
subtitle: 포스트 작성 중
gh-repo: gyunrinkle/hello-spring
gh-badge: [star, fork, follow]
categories: [web-dev]
tags: [spring-boot, java]
comments: true
sitemap:
  changefreq: daily
---

`build.gradle`
```groovy
implementation 'org.projectlombok:lombok'  
// annotation processor를 등록해야 배포가 됨  
annotationProcessor 'org.projectlombok:lombok'
```