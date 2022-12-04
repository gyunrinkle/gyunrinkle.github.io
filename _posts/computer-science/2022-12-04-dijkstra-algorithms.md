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

# 다익스트라 알고리즘은 뭘까?

나무위키에서 다음과 같이 나온다.

> 음의 가중치가 없는 [그래프](https://namu.wiki/w/%EA%B7%B8%EB%9E%98%ED%94%84(%EC%9D%B4%EC%82%B0%EC%88%98%ED%95%99) "그래프(이산수학)")의 한 정점(頂點, Vertex)에서 모든 정점까지의 최단거리를 각각 구하는 [알고리즘](https://namu.wiki/w/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "알고리즘")(최단 경로 문제, Shortest Path Problem)이다.  
>
> [에츠허르 다익스트라](https://namu.wiki/w/%EC%97%90%EC%B8%A0%ED%97%88%EB%A5%B4%20%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC "에츠허르 다익스트라")가 고안한 알고리즘으로, 그가 처음 고안한 알고리즘은 O(V^2)O(V2)의 시간복잡도를 가졌다. 이후 [우선순위 큐](https://namu.wiki/w/%ED%81%90(%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0)#s-3.2 "큐(자료구조)")(=[힙 트리](https://namu.wiki/w/%ED%9E%99%20%ED%8A%B8%EB%A6%AC "힙 트리"))등을 이용한 더욱 개선된 알고리즘이 나오며, O((V+E)logV)O((V+E)logV)(V는 정점의 개수, E는 한 정점의 주변 노드)의 시간복잡도를 가지게 되었다.[[1]](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#fn-1)  
>
> O((V+E)logV)O((V+E)logV)의 시간복잡도를 가지는 이유는 각 노드마다 미방문 노드 중 출발점으로부터 현재까지 계산된 최단 거리를 가지는 노드를 찾는데 O(VlogV)O(VlogV)의 시간이 필요하고[[2]](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#fn-2), 각 노드마다 이웃한 노드의 최단 거리를 갱신할 때 O(ElogV)O(ElogV)의 시간이 필요하기 때문이다.[[3]](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#fn-3)  
>
> 그래프 방향의 유무는 상관 없으나, 간선(幹線, Edge)들 중 단 하나라도 가중치가 음수이면 이 알고리즘은 사용할 수 없다. 음의 가중치를 가지는 간선이 있으며, 가중치 합이 음인 사이클이 존재하지 않는 경우 [벨만-포드 알고리즘](https://namu.wiki/w/%EB%B2%A8%EB%A7%8C-%ED%8F%AC%EB%93%9C%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "벨만-포드 알고리즘")을 사용 가능하다. 또한 그래프 내에 가중치 합이 음인 사이클이 존재한다면 무한히 음의 사이클을 도는 경우 경로 합이 음수 무한대로 발산하기 때문에 그래프 내의 최단 경로는 구성할 수 없다.  
>
> 다익스트라 알고리즘이 하나의 노드로부터 최단경로를 구하는 알고리즘인 반면, [플로이드-워셜 알고리즘](https://namu.wiki/w/%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EC%9B%8C%EC%85%9C%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "플로이드-워셜 알고리즘")은 가능한 모든 노드쌍들에 대한 최단거리를 구하는 알고리즘이다.[[4]](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#fn-4)  
>
> 다익스트라 알고리즘을 확장시킨 알고리즘이 [A* 알고리즘](https://namu.wiki/w/A*%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98 "A* 알고리즘")이다.

O(V2)
>O(V2