---
layout: post
title: 다익스트라 알고리즘
subtitle: 최단거리 알고리즘
categories: [computer-science]
tags: [dijkstra, greedy, priority-queue]
comments: true
sitemap:
  changefreq: daily
---

# 다익스트라 알고리즘이란?

정의는 [나무위키](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)나 [위키백과](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)를 확인해보자. 아니면 유명한 전공 서적인 `Introduction to Algorithms`에서도 친절히 정의가 나와있다.

# 다익스트라 알고리즘 그림 설명으로 확인 해보자

예를 들어, 다음과 같은 네트워크가 존재한다고 가정하자. 먼저, A 라우터의 목표는 F까지의 최단 거리 계산이며, 수단으로는 다익스트라 알고리즘을 활용한다. 이때, 각 데이터의 의미는 다음과 같다.

-   `S = 방문한 노드들의 집합`
-   `d[N] = A → N까지 계산된 최단 거리`
-   `Q = 방문하지 않은 노드들의 집합`

# 1. 아직 확인되지 않은 거리는 모두 무한대

** 다익스트라 알고리즘은 아직 확인되지 않은 거리는 전부 초기값을 무한으로 잡는다. **

![초기화 실행된 후의 그래프](https://w.namu.la/s/7cff087eb1f8876860f0d7a5e1747bd52eb9e20faff468bf3dbb9b267bd14a82602df9a6ef657a6bec140570b00efa1d8779c96fc3a6af1e9075f84ce3493c53fed6d64a6b3ccaf9ea96187dc704e731cff68c20cbed99152f27d2e5c17ace3c)
