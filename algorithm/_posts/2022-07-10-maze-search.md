---
layout: post
title: "[백준 - 2178] 미로 찾기"
description: >
  백준 Class3 Silver1
sitemap: false
tags: [BFS]
hide_last_modified: true
---

# 🔎 미로 탐색

solved_ac[Class3] [미로 탐색](https://www.acmicpc.net/problem/2178)

## 문제
N×M크기의 배열로 표현되는 미로가 있다.

<img width="209" alt="스크린샷 2022-07-07 오후 12 59 39" src="https://user-images.githubusercontent.com/88064555/177687751-fcc6098f-2df5-4121-932f-24d79340ce57.png">

미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. 이러한 미로가 주어졌을 때, (1, 1)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 최소의 칸 수를 구하는 프로그램을 작성하시오. 한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다.

위의 예에서는 15칸을 지나야 (N, M)의 위치로 이동할 수 있다. 칸을 셀 때에는 시작 위치와 도착 위치도 포함한다.

## 입력
첫째 줄에 두 정수 N, M(2 ≤ N, M ≤ 100)이 주어진다. 다음 N개의 줄에는 M개의 정수로 미로가 주어진다. 각각의 수들은 **붙어서** 입력으로 주어진다.

## 출력
첫째 줄에 지나야 하는 최소의 칸 수를 출력한다. 항상 도착위치로 이동할 수 있는 경우만 입력으로 주어진다.

## 예제 입력 1 

```
4 6
101111
101010
101011
111011
```

## 예제 출력 1 

```
15
```

## 예제 입력 2

```
4 6
110110
110110
111111
111101
```

## 예제 출력 2

```
9
```

## 예제 입력 3

```
2 25
1011101110111011101110111
1110111011101110111011101
```

## 예제 출력 3

```
38
```

## 예제 입력 4

```
7 7
1011111
1110001
1000001
1000001
1000001
1000001
1111111
```

## 예제 출력 4

```
13
```

# 🛠 자료구조

이 문제는 **BFS**를 사용해야 한다. 왜냐하면 **"특정 노드에서 특정 노드까지의 최단 거리 & Weight 없음"**이기 때문이다.

<img width="466" alt="스크린샷 2022-07-10 오전 7 08 47" src="https://user-images.githubusercontent.com/88064555/178124143-107fbff1-e499-44de-9732-f59b54816e8a.png">

BFS로 그래프 전체를 탐색하며 (0,0) 위치로부터 이동 칸 수를 입력하고 최종 탐색이 종료되었을 때 (N-1,M-1)위치의 이동 칸 수를 출력한다.

문제에서는 시작점이 (1,1) 종료점이 (N,M)이지만 배열의 index가 (0,0)부터 시작하므로 이를 고려하였다.

# 👨🏻‍💻 CODE

```python
from collections import deque
N,M = map(int,input().split())

# 미로 초기화
graph = [[0]*M for _ in range(N)]

# 미로 정보 입력
for i in range(N):
    line = input()
    for j in range(M):
       graph[i][j] = int(line[j])

dx = [1,-1,0,0]
dy = [0,0,1,-1]

# BFS
def bfs(x,y):
    q = deque([(x,y)])
    
    while q:
        cur = q.popleft()
        ox,oy = cur[0],cur[1]

        # 정답을 찾은 Case
        if cur == (N-1,M-1):
            break

        # 정답을 찾아가는 과정
        for i in range(4):
            nx = cur[0]+dx[i]
            ny = cur[1]+dy[i]

            if 0<=nx<N and 0<=ny<M and graph[nx][ny] == 1:
                q.append((nx,ny))
                graph[nx][ny] = graph[ox][oy] + 1

bfs(0,0)

print(graph[N-1][M-1])
```

