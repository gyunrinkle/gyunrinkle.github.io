---
layout: post
title: Spring Boot에서 CORS 적용하기
subtitle: 어노테이션 한 방이면 끝
gh-repo: gyunrinkle/hello-spring
gh-badge: [star, fork, follow]
categories: [web-dev]
tags: [spring-boot, java]
comments: true
sitemap:
  changefreq: daily
---
`controller.java`에 가서 
```java
@CrossOrigin("*") // 모든 요청에 접근 허용
@RestController
```