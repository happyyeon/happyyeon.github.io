---
layout: post
title: "[백준 - 1504] 특정한 최단 경로"
description: >
  백준 Class4 Gold4
sitemap: false
tags: [Dijkstra]
hide_last_modified: true
---

# 🚙 특정한 최단 경로

solved_ac[Class4] [특정한 최단 경로](https://www.acmicpc.net/problem/1504)

## 문제
---
방향성이 없는 그래프가 주어진다. 세준이는 1번 정점에서 N번 정점으로 최단 거리로 이동하려고 한다. 또한 세준이는 두 가지 조건을 만족하면서 이동하는 특정한 최단 경로를 구하고 싶은데, 그것은 바로 임의로 주어진 두 정점은 반드시 통과해야 한다는 것이다.

세준이는 한번 이동했던 정점은 물론, 한번 이동했던 간선도 다시 이동할 수 있다. 하지만 반드시 최단 경로로 이동해야 한다는 사실에 주의하라. 1번 정점에서 N번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치면서 최단 경로로 이동하는 프로그램을 작성하시오.

## 입력
---
첫째 줄에 정점의 개수 N과 간선의 개수 E가 주어진다. (2 ≤ N ≤ 800, 0 ≤ E ≤ 200,000) 둘째 줄부터 E개의 줄에 걸쳐서 세 개의 정수 a, b, c가 주어지는데, a번 정점에서 b번 정점까지 양방향 길이 존재하며, 그 거리가 c라는 뜻이다. (1 ≤ c ≤ 1,000) 다음 줄에는 반드시 거쳐야 하는 두 개의 서로 다른 정점 번호 v1과 v2가 주어진다. (v1 ≠ v2, v1 ≠ N, v2 ≠ 1) 임의의 두 정점 u와 v사이에는 간선이 최대 1개 존재한다.

## 출력
---
첫째 줄에 두 개의 정점을 지나는 최단 경로의 길이를 출력한다. 그러한 경로가 없을 때에는 -1을 출력한다.

## 예제 입력 1 

```
4 6
1 2 3
2 3 3
3 4 1
1 3 5
2 4 5
1 4 4
2 3
```

## 예제 출력 1 

```
7
```

# 📖 알고리즘

⭐️**[다익스트라 알고리즘은 Weight가 있는 그래프에서 <특정 노드 ➡️ 모든 노드> 최소 비용을 구할 때 사용한다.]**⭐️

이 문제를 분석해보면 다음과 같다.

+ Weight가 존재하는 그래프
+ 노드 1(특정 노드)에서 노드 N으로 가는 최단 경로(최소 비용)을 구해야 한다.

다익스트라를 이용하면 노드 1에서부터 모든 노드의 최소 비용이 구해질테고 우리는 노드 N(N번 인덱스)만 추출하여 이용하면 정답을 맞출 수 있다.

## Heap을 이용한 다익스트라 기본 골격 코드

```python
## 문제 조건 세팅 ##
N,E = map(int,input().split())
graph = [[] for _ in range(N+1)]

for i in range(E):
    a,b,c = map(int,input().split())
    graph[a].append((b,c))
    graph[b].append((a,c))

v1,v2 = map(int,input().split())
####

def dijkstra(start):
    distance = [INF]*(N+1)
    heap = []
    h.heappush(heap,(0,start))
    distance[start] = 0

    while heap:
        dist,now = h.heappop(heap)

        if distance[now] < dist:
            continue

        for i in graph[now]:
            cost = dist + i[1]

            if cost < distance[i[0]]:
                distance[i[0]] = cost
                h.heappush(heap,(cost,i[0]))
    
    return distance
```

# 📐 설계

1. 노드 1 ➡️ 노드 v1 ➡️ 노드 v2 ➡️ 노드 N
2. 노드 1  ➡️ 노드 v2 ➡️ 노드 v1 ➡️ 노드 N

1번과 2번 중 최소 출력

## ⚠️ 주의사항

마지막 예외처리 조건 

```python
if answer == INF:
```
로 하면 틀린다.

왜냐하면 노드 1 ➡️ 노드 v1 = INF , 노드 v1 ➡️ 노드 v2 = INF , 노드 v2 ➡️ 노드 N = INF 일 경우에

answer의 값은 3*INF가 되기 때문이다. 따라서 예외처리는

```python
if answer >= INF:
```
로 해주어야 한다.

# 👨🏻‍💻 CODE

```python
import heapq as h
import sys
input = sys.stdin.readline
INF = int(1e9)
## 문제 조건 세팅 ##
N,E = map(int,input().split())
graph = [[] for _ in range(N+1)]

for i in range(E):
    a,b,c = map(int,input().split())
    graph[a].append((b,c))
    graph[b].append((a,c))

v1,v2 = map(int,input().split())
####

# 다익스트라 알고리즘
# 특정 노드 --> 특정 노드 최단 경로를 구하기 위하여
# 시간복잡도를 줄이기 위하여 힙을 사용
def dijkstra(start):
    distance = [INF]*(N+1)
    heap = []
    h.heappush(heap,(0,start))
    distance[start] = 0

    while heap:
        dist,now = h.heappop(heap)

        if distance[now] < dist:
            continue

        for i in graph[now]:
            cost = dist + i[1]

            if cost < distance[i[0]]:
                distance[i[0]] = cost
                h.heappush(heap,(cost,i[0]))
    
    return distance

# === 노드 1 --> 노드 N (v1,v2 경유) ===
# 노드 1 --> 노드 v1 최단 경로
# 노드 v1 --> 노드 v2 최단 경로
# 노드 v2 --> 노드 N 최단 경로
# =======================================

sum1 = dijkstra(1)[v1] + dijkstra(v1)[v2] + dijkstra(v2)[N]

sum2 = dijkstra(1)[v2] + dijkstra(v2)[v1] + dijkstra(v1)[N]

answer = min(sum1,sum2)

if answer >= INF:
    print(-1)
else:
    print(answer)
```









