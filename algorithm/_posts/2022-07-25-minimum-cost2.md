---
layout: post
title: "[백준 - 11779] 최소비용 구하기 2"
description: >
  백준 Gold3
sitemap: false
tags: [Dijkstra]
hide_last_modified: true
---

# 🚌 최소비용 구하기 2

[최소비용 구하기 2](https://www.acmicpc.net/problem/11779)

## 문제

n(1≤n≤1,000)개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 m(1≤m≤100,000)개의 버스가 있다. 우리는 A번째 도시에서 B번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 한다. 그러면 A번째 도시에서 B번째 도시 까지 가는데 드는 최소비용과 경로를 출력하여라. 항상 시작점에서 도착점으로의 경로가 존재한다.

## 입력

첫째 줄에 도시의 개수 n(1≤n≤1,000)이 주어지고 둘째 줄에는 버스의 개수 m(1≤m≤100,000)이 주어진다. 그리고 셋째 줄부터 m+2줄까지 다음과 같은 버스의 정보가 주어진다. 먼저 처음에는 그 버스의 출발 도시의 번호가 주어진다. 그리고 그 다음에는 도착지의 도시 번호가 주어지고 또 그 버스 비용이 주어진다. 버스 비용은 0보다 크거나 같고, 100,000보다 작은 정수이다.

그리고 m+3째 줄에는 우리가 구하고자 하는 구간 출발점의 도시번호와 도착점의 도시번호가 주어진다.

## 출력

첫째 줄에 출발 도시에서 도착 도시까지 가는데 드는 최소 비용을 출력한다.

둘째 줄에는 그러한 최소 비용을 갖는 경로에 포함되어있는 도시의 개수를 출력한다. 출발 도시와 도착 도시도 포함한다.

셋째 줄에는 최소 비용을 갖는 경로를 방문하는 도시 순서대로 출력한다.

## 예제 입력 1 

```
5
8
1 2 2
1 3 3
1 4 1
1 5 10
2 4 2
3 4 1
3 5 1
4 5 3
1 5
```

## 예제 출력 1 

```
4
3
1 3 5
```

# 📖 알고리즘

![연습장-6](https://user-images.githubusercontent.com/88064555/180663445-c70e61f6-89af-4a3a-a815-199b97ec0c9d.jpg)

예제를 그림으로 표현해보면 위와 같다. 

버스 비용이 존재하는 노선도에서 A 도시에서 B 도시로 가는 최소 비용을 출력하는 문제이다.

이를 일반화 하면 **[Weight가 존재하는 그래프에서 특정 노드에서 특정 노드로 가는 최소 비용을 묻는]** 문제로 바라볼 수 있다. 바로 다익스트라를 갖다 써주면 된다.

다익스트라 기본 문제에서 경로를 저장하여 출력해준다는 조건만 추가하면 된다.

# 👨🏻‍💻 CODE

```python
import heapq as h
import sys
input = sys.stdin.readline
INF = int(1e9)
city = int(input())
bus = int(input())

graph = [[] for _ in range(city+1)]

for i in range(bus):
    a,b,c = map(int,input().split())
    graph[a].append((b,c))

start,end = map(int,input().split())
####



def dijkstra(start):
    distance = [INF]*(city+1)
    heap = []
    h.heappush(heap,(0,start))
    distance[start] = 0
    path = [[] for _ in range(city+1)] # 경로
    path[start].append(start)

    while heap:
        dist,now = h.heappop(heap)

        if distance[now] < dist:
            continue

        for i in graph[now]:
            cost = dist + i[1]

            if cost < distance[i[0]]:
                distance[i[0]] = cost
                h.heappush(heap,(cost,i[0]))
                path[i[0]] = []
                for j in path[now]:
                    path[i[0]].append(j)
                path[i[0]].append(i[0])
    
    return distance,path

distance,path = dijkstra(start)
print(distance[end])
print(len(path[end]))
print(" ".join(map(str,path[end])))
```



