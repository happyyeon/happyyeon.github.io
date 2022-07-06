---
layout: post
title: 최소비용 구하기
sitemap: false
tags: [Dijkstra]
hide_last_modified: true
---
# 문제 설명

<img width="1175" alt="스크린샷 2022-05-11 오후 2 20 27" src="https://user-images.githubusercontent.com/88064555/167774544-b25f09b8-b14b-44a1-805d-f6fd61b7e5e2.png">

# 알고리즘 분류

**[다익스트라 알고리즘]**

이 문제에서 왜 다익스트라를 사용해야 할까? 그 이유는 다음과 같다.

<img width="1247" alt="스크린샷 2022-05-11 오후 2 27 04" src="https://user-images.githubusercontent.com/88064555/167775314-c1cefd77-cb4d-4063-913a-80ed66e2e8ab.png">

출발 도시에서 도착 도시로 가는 것이니 단일 노드 - 단일 노드이고

버스 비용이 발생하고 이는 양수이니까 Edge에 양수 weight가 있는 카테고리로 분류가 된다.

# 입출력 예시

<img width="485" alt="스크린샷 2022-05-11 오후 2 30 34" src="https://user-images.githubusercontent.com/88064555/167775641-0cd03a38-2845-4bab-9618-34d056ffb317.png">

# 다익스트라 알고리즘

## Pseudo CODE

1. 힙에 (비용,현재 노드) push
2. while 힙
    
    + 2-0. heappop --> 현재 마을까지 걸린 시간,현재 노드

    + 2-1. if 현재 마을까지 걸린 시간 > 현재 마을의 최소비용:

        이건 무의미하므로 제낌

    + 2-2. 현재노드와 연결된 모든 간선으로부터:

        + 2-2-1. if 다음 노드까지의 비용 < 다음 마을의 최소비용:

            다음 마을의 최소비용 업데이트
            
            다음 마을 heappush

## Python CODE
```python
def dijkstra(start):
    q = []
    heapq.heappush(q,(0,start))
    distance[start] = 0
    while q:
        dist, now = heapq.heappop(q) # (이전->현재) 걸린 시간, 현재 마을
        if dist > distance[now]: # distance = 출발 마을에서 index 마을까지 걸리는 최소 시간
            continue

        for i in cities[now]:
            cost = dist + i[1] # (이전->현재->다음) 걸릴 시간
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q,(cost,i[0]))
    return distance
```
## 왜 다익스트라 알고리즘에서 heap을 사용할까?

![Minimum_Cost-3](https://user-images.githubusercontent.com/88064555/167823618-5cef8642-55a4-40fa-a9d5-d6da3e6c7eec.jpg)

파이썬에서 힙모듈은 최소힙이다. 따라서 (비용,노드) 쌍으로 힙을 생성하면 루트노드는 언제나 최소비용이다.

다익스트라에서 중요한 포인트는 최소비용으로 목표 노드까지 도달하는 것이기 때문에 힙을 사용하여 최소비용을 갱신한다.

## 전체 소스 코드

```python
import heapq
import sys
input = sys.stdin.readline
INF = int(1e9)
# 초기 조건
city = int(input())
bus = int(input())
# 도시 --> 그래프로 표현
cities = [[] for _ in range(city+1)]
for i in range(bus):
    a,b,c = map(int,input().split())
    cities[a].append([b,c])
start,end = map(int,input().split()) # 출발지, 도착지

# "출발 노드 --> 각 노드" 최소 비용
distance = [INF]*(city+1)

# 다익스트라 알고리즘
def dijkstra(start):
    q = []
    heapq.heappush(q,(0,start))
    distance[start] = 0
    while q:
        dist, now = heapq.heappop(q) # (이전->현재) 걸린 시간, 현재 마을
        if dist > distance[now]: # distance = 출발 마을에서 index 마을까지 걸리는 최소 시간
            continue

        for i in cities[now]:
            cost = dist + i[1] # (이전->현재->다음) 걸릴 시간
            if cost < distance[i[0]]:
                distance[i[0]] = cost
                heapq.heappush(q,(cost,i[0]))
    return distance

# 정답 구하기
answer_list = dijkstra(start)
print(answer_list[end])
```

