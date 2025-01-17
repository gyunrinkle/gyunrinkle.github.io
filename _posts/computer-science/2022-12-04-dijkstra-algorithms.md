---
layout: post
title: 다익스트라 알고리즘
subtitle: 최단거리 알고리즘
categories: [computer-science]
tags: [algorithms, dijkstra, greedy]
comments: true
sitemap:
  changefreq: daily
---

# 다익스트라 알고리즘이란?

정의는 [나무위키](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)나 [위키백과](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%81%AC%EC%8A%A4%ED%8A%B8%EB%9D%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)를 확인해보자. 아니면 유명한 전공 서적인 `Introduction to Algorithms`에서도 친절히 정의가 나와있다. 단 중요한 조건이 한 개가 있다.

- **간선의 가중치가 단 한 개라도 음수이면 안된다.**

# 다익스트라 알고리즘 그림 설명으로 확인 해보자

예를 들어, 다음과 같은 네트워크가 존재한다고 가정하자. 먼저, A 라우터의 목표는 F까지의 최단 거리 계산이며, 수단으로는 다익스트라 알고리즘을 활용한다. 이때, 각 데이터의 의미는 다음과 같다.

- `S = 방문한 노드들의 집합`
- `d[N] = A → N까지 계산된 최단 거리`
- `Q = 방문하지 않은 노드들의 집합`

# 1. **아직 확인되지 않은 거리는 모두 무한대**

**다익스트라 알고리즘은 아직 확인되지 않은 거리는 전부 초기값을 무한으로 잡는다.**

![초기화 실행된 후의 그래프](https://w.namu.la/s/7cff087eb1f8876860f0d7a5e1747bd52eb9e20faff468bf3dbb9b267bd14a82602df9a6ef657a6bec140570b00efa1d8779c96fc3a6af1e9075f84ce3493c53fed6d64a6b3ccaf9ea96187dc704e731cff68c20cbed99152f27d2e5c17ace3c)

- 출발지를 A로 선택하고, `d`을 초기화했다. 그러므로 `d[A]`는 출발지 - 출발지 사이 거리이므로 0이 돼야 한다. (자기 자신한테 가는 최단 거리는 0이다.)
- 출발지를 제외한 모든 노드들은 아직 방문하지 않았기에, `d[다른 노드] = 무한`이 된다.
- Q는 방문하지 않은 노드들의 집합이므로, 초기화할 때는 모든 노드들의 집합이 된다.
- S는 공집합 상태이다.

# 2.**루프를 돌려 이웃 노드 방문 후 최단 거리 계산**

**루프를 돌려 이웃 노드를 방문하고 거리를 계산한다.**

![첫 루프를 마치고 난 뒤의 그래프](https://w.namu.la/s/4e98f57a9b80f41aad785aa08b05c88f1380e88f3351d17c0145227fc4c69e39db4573bc6097fa7189ca455584a1ce96b9fa274bc3fbb672a8d960ceb070b7919627fbfef150a11abe17106e6b958967b4a7b3ba4d5fed73c39c41b6bcbaf23d)

- 루프의 시작은 거리가 최소인 노드(`d[N]`이 최소값인 노드 N)를 Q(방문하지 않은 노드의 집합)에서 제거하고, 그 노드를 S(방문한 노드의 집합 및 최소 경로)에 추가한다. 즉, N을 '방문한다'는 의미이다.
- 이후, 모든 이웃 노드와의 거리를 측정하여 `d[N](=출발지로부터 N까지 계산된 최소 거리값) + (N과 이웃 노드 간의 거리값) = (출발지부터 이웃 노드까지의 거리값)`이 계산된다.
- 다만, 지금은 첫 번째 루프만을 끝낸 상태이므로, 단순히 0 + (이웃 노드와 출발지 사이의 거리값) = (출발지와 이웃 노드 간의 거리값)이 각 이웃 노드에 기록된다. 따라서, `d[B] = 10, d[C] = 30, d[D] =15`로 값을 변경한다.

# 3.**첫 루프 이후의 그래프의 상태가 업데이트되는 방식**

이제 루프가 반복적으로 작동하는 방법을 설명한다. 밑의 그림들 또한 루프 안에서 알고리즘이 진행되는 순간들을 반복 설명한다.

![](https://w.namu.la/s/a915731233ba006e765c8bce2fd56cdb0dda05fe2c3cab1020b4f0a3031d58d208973d4034e6e1d8e0bf73a8aeabec275b163417c9d7cdac22080413e1e126a2d4b5ef57046844ce0f443cabcd3f9e168a43bdca973f23fd72c695bed314eeda)

- 이전에 설명했듯이, 방문할 노드는 Q에 남아있는 노드들 중 `d[N]` 값이 제일 작은 것으로 선택된다. 따라서, `Q = {B, C, D, E, F}` 중 B가 `d[B] = 10`으로 제일 작은 값을 가지므로, B를 방문하여 S에 추가하고 Q에서 제거한다.
- 이제, B의 이웃 노드들을 모두 탐색하여 거리를 재고 `d[N]`에 기록한다. B의 이웃 노드는 E뿐이므로, `d[E]` 값이 무한에서 `d[B]+(B와 E 사이의 값 20) = 30` 으로 업데이트된다. 여기서 `d[B]` 값을 더하는 이유는 **출발지부터의** 거리값을 재기 위해서다.

# 4.**더 빠른 경로를 발견한 경우**

![](https://w.namu.la/s/12e2bca491edeed1c5d1e6c9b5c13fd91973b580d68f8ff3a0997395d82f025ae46b7e5a1f66d67bc63127f9a742ddbe748e9c6cdd27faaa16bde9f88ab9855278ac766a7f62b0578d9c21e60f687c6629b8d000fe977014d2a90188013883f2)

- 3번째 그림에서 설명했듯이, 이번에 선택·방문되는 노드는 D이다. Q의 원소 중에서 제일 낮은 `d[N]` 값을 가지고 있기 때문이다. 그래서, S에 D가 추가되어 있다.
- 이제 D의 이웃 노드들(C, F)의 거리를 잰 후, `d[N]`값을 업데이트한다. 특별한 상황은 `d[C]`의 값이 A를 방문할 때 이미 계산되어 30으로 정해져 있었으나, D를 방문하여 C와의 거리를 확인해 보니 20으로 **더 짧은 최단 경로가 발견되었다!** 그러므로, `d[C]`의 값을 30에서 20으로 변경한다.
- `d[F]`의 경우는 원래의 값이 무한이므로, 더 작은 값인 15+20=35로 변경한다.
-

# 5.**또 다른 반복 루프 상황**

![](https://w.namu.la/s/d6fbe1219d765106e87b61d0bf3ffbe1d0398ed40aee660dc430e30b854e8d62cdfbe5a47c40d8589dd959b2c327fbe58cfb56492b9d6a1249e79b89f346df451eccfc3adc40613f5e1a5d9b596eb25d59fccaa28881af09e57a626f9f768dc6)

- Q = {C,E,F}에서 `d[C] = 20`으로 C를 방문하여, S는 {A, B, D, C}가 되었다.

- 다시 이웃 노드와의 거리 계산을 해보니, 이번에는 (A→D) + (D→F) = 15 + 20 = 35보다, (A→D) + (D→C) + (C→F) = 15 + 5 + 5 = **25**로 더 작은 값을 가지는 것이 발견되었다. `d[F] = 25`로 업데이트한다.

이 일련의 과정이 반복되어 Q가 공집합이 되었다면, 남아 있는 데이터로 추론하여 최단 거리를 결정한다.

마지막 루프 이후,

```
-   S = {A, B, D, C, F, E} (방문한 순서대로 정렬)
-   d[A] = 0
-   d[B] = 10
-   d[C] = 20
-   d[D] = 15
-   d[E] = 30
-   d[F] = 25
-   Q = ∅
```

목적지가 F였으므로, A→D→C→F가 최단 경로이며, 거리는 25로 결정된다.

# Python Code

```python
infinity = 10 ** 9  # 무한을 대충 10억으로 잡는다

S = set()  # 방문한 노드들의 집합
Q = set(['A', 'B', 'C', 'D', 'E', 'F'])  # 방문안한 노드들의 집합
d = {'A': infinity, 'B': infinity, 'C': infinity, 'D': infinity, 'E': infinity,
     'F': infinity}  # 최단 거리를 메모하는 딕셔너리 (초기 값은 모두 무한대)
G = {'A': [('B', 10), ('C', 30), ('D', 15)], 'B': [('E', 20)], 'C': [('F', 5)], 'D': [('C', 5), ('F', 20)],
     'E': [('F', 20)], 'F': [('D', 20)]}  # 그래프에서 노드들의 관계를 나타낸 딕셔너리


def dijkstra(source):
    # 출발지에서 출발지까지의 최단 거리는 0이다.
    d[source] = 0

    # Q가 공집합이 아니면
    while Q:
        # Q의 남아 있는 원소들 중에서 가장 작은 최단 거리를 가진 노드를 u라고 선언
        u = min({q: d[q] for q in Q}, key=lambda x: d[x])

        # u를 Q에서 제거
        Q.remove(u)
        # u를 S에 추가
        S.add(u)

        for neighbor, w in G[u]:
            alt = d[u] + w
            if alt < d[neighbor]:
                d[neighbor] = alt


dijkstra('A')
print(f'S: {S}')
print(f'Q: {Q}')
print(f'd: {d}')

```

# 출처

[나무위키](https://namu.wiki/w/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#s-3.2)
