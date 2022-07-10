---
layout: post
title: "[백준 - 11403] 경로 찾기"
description: >
  백준 Class3 Silver1
sitemap: false
tags: [BFS]
hide_last_modified: true
---

# 🔎 경로 찾기

solved_ac[Class3] [경로 찾기](https://www.acmicpc.net/problem/11403)

## 문제
가중치 없는 방향 그래프 G가 주어졌을 때, 모든 정점 (i, j)에 대해서, i에서 j로 가는 경로가 있는지 없는지 구하는 프로그램을 작성하시오.

## 입력
첫째 줄에 정점의 개수 N (1 ≤ N ≤ 100)이 주어진다. 둘째 줄부터 N개 줄에는 그래프의 인접 행렬이 주어진다. i번째 줄의 j번째 숫자가 1인 경우에는 i에서 j로 가는 간선이 존재한다는 뜻이고, 0인 경우는 없다는 뜻이다. i번째 줄의 i번째 숫자는 항상 0이다.

## 출력
총 N개의 줄에 걸쳐서 문제의 정답을 인접행렬 형식으로 출력한다. 정점 i에서 j로 가는 경로가 있으면 i번째 줄의 j번째 숫자를 1로, 없으면 0으로 출력해야 한다.

## 예제 입력 1 

```
3
0 1 0
0 0 1
1 0 0
```

## 예제 출력 1 

```
1 1 1
1 1 1
1 1 1
```

## 예제 입력 2

```
7
0 0 0 1 0 0 0
0 0 0 0 0 0 1
0 0 0 0 0 0 0
0 0 0 0 1 1 0
1 0 0 0 0 0 0
0 0 0 0 0 0 1
0 0 1 0 0 0 0
```

## 예제 출력 2

```
1 0 1 1 1 1 1
0 0 1 0 0 0 1
0 0 0 0 0 0 0
1 0 1 1 1 1 1
1 0 1 1 1 1 1
0 0 1 0 0 0 1
0 0 1 0 0 0 0
```

# 🛠 자료구조

![연습장-12](https://user-images.githubusercontent.com/88064555/178127142-417c9265-22ce-4659-a3f2-b0c294c54fa3.jpg)

Visited 배열 ➡️ index 노드를 방문하였는지를 알려주는 배열

이 문제가 묻고자 하는 요지를 한 마디로 정리하면 

**"특정 노드로부터 그래프 탐색을 진행하였을 때, 방문할 수 있는 노드는?"**

따라서, 0번 노드부터 N-1번 노드까지 각각 그래프 탐색을 하여 Visited 배열을 뽑아주면 된다.

# 👨🏻‍💻 CODE

```python
from collections import deque

N = int(input())
graph = [[0]*N for _ in range(N)]

for i in range(N):
    line = input().replace(" ","")
    for j in range(N):
        graph[i][j] = int(line[j])

def bfs(start):
    visited = [0]*N
    q = deque([start])

    while q:
        cur = q.popleft()

        for i in range(N):
            if graph[cur][i] == 1 and visited[i] == 0:
                q.append(i)
                visited[i] = 1
    
    return visited

for node in range(N):
    print(*bfs(node))
```

