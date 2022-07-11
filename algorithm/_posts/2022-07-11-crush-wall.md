---
layout: post
title: "[백준 - 2206] 벽 부수고 이동하기"
description: >
  백준 Class4 Gold4
sitemap: false
tags: [BFS,구현]
hide_last_modified: true
---

# 🔨 벽 부수고 이동하기

solved_ac[Class4] [벽 부수고 이동하기](https://www.acmicpc.net/problem/2206)

## 문제
---
N×M의 행렬로 표현되는 맵이 있다. 맵에서 0은 이동할 수 있는 곳을 나타내고, 1은 이동할 수 없는 벽이 있는 곳을 나타낸다. 당신은 (1, 1)에서 (N, M)의 위치까지 이동하려 하는데, 이때 최단 경로로 이동하려 한다. 최단경로는 맵에서 가장 적은 개수의 칸을 지나는 경로를 말하는데, 이때 시작하는 칸과 끝나는 칸도 포함해서 센다.

만약에 이동하는 도중에 한 개의 벽을 부수고 이동하는 것이 좀 더 경로가 짧아진다면, 벽을 한 개 까지 부수고 이동하여도 된다.

한 칸에서 이동할 수 있는 칸은 상하좌우로 인접한 칸이다.

맵이 주어졌을 때, 최단 경로를 구해 내는 프로그램을 작성하시오.

## 입력
---
첫째 줄에 N(1 ≤ N ≤ 1,000), M(1 ≤ M ≤ 1,000)이 주어진다. 다음 N개의 줄에 M개의 숫자로 맵이 주어진다. (1, 1)과 (N, M)은 항상 0이라고 가정하자.

## 출력
---
첫째 줄에 최단 거리를 출력한다. 불가능할 때는 -1을 출력한다.

## 예제 입력 1 

```
6 4
0100
1110
1000
0000
0111
0000
```

## 예제 출력 1 

```
15
```

## 예제 입력 2

```
4 4
0111
1111
1111
1110
```

## 예제 출력 2

```
-1
```

# 📖 자료구조

[BFS + 구현]이었다. 다만 그 구현 부분이 무척 어려웠다. 

## TRY 1

<img width="853" alt="스크린샷 2022-07-11 오후 7 05 57" src="https://user-images.githubusercontent.com/88064555/178240935-c672b45c-1efa-45bb-9813-e3a3ae7a4339.png">

처음에는 Brute Force로 접근하였다. 모든 벽에 대하여 한 번씩 제거해보며 제일 최단 경로를 출력하는 것이다.

👉🏻 시간 초과

## TRY 2

![연습장-14](https://user-images.githubusercontent.com/88064555/178241604-65ea94db-6800-4f3f-8acd-58dda0023958.jpg)

![연습장-15](https://user-images.githubusercontent.com/88064555/178241706-d382cb28-8f17-4564-a8eb-28907f435b57.jpg)

![연습장-16](https://user-images.githubusercontent.com/88064555/178241763-f43e71c5-7d81-42b0-9098-2a9b0ee03f37.jpg)

"TRY 1"에서 시간 초과가 나는 이유는 시간복잡도가 O(N^2)이기 때문이다. 이를 줄여야 하는데 도저히 생각이 안 났다.

동혁이가 <3차원>이라는 힌트를 줘서 이를 바탕으로 다시 생각을 해 보았다.

* 벽을 안 부수는 경우의 맵
* 벽을 부수는 경우의 맵

이렇게 두 가지로 나누어서 탐색을 진행하면 되지 않을까 하고 진행 과정을 그려보았다.

길 혹은 벽 오른쪽에 있는 분홍색 숫자가 경로의 길이를 의미한다.

하지만 이렇게 했을 경우 벽을 2번 이상 부수게 되는 경우가 발생하여 오답이다.

👉🏻 오답

## TRY 3

![연습장-17](https://user-images.githubusercontent.com/88064555/178242992-8ecb787c-4949-41f0-b21c-cd26ddb003da.jpg)

![연습장-18](https://user-images.githubusercontent.com/88064555/178243059-afa35bed-7459-4a68-a5f3-c7c123e18825.jpg)

![연습장-19](https://user-images.githubusercontent.com/88064555/178243119-9a3eedde-920a-45a0-bcd7-0edd6ed672fe.jpg)

시간 관계상 결국 솔루션을 참고하였다. 
<https://hongcoding.tistory.com/18>

# 👨🏻‍💻 CODE

## TRY 1

```python
import sys
from collections import deque
import copy
input = sys.stdin.readline
N,M = map(int,input().split())


graph = [[0]*M for _ in range(N)]

# 맵 생성
for i in range(N):
    line = input()
    for j in range(M):
        graph[i][j] = int(line[j])

dx = [1,-1,0,0]
dy = [0,0,1,-1]

# BFS로 최단거리 탐색
def bfs(a,b,graph):
    q = deque([(a,b)])

    while q:
        x,y = q.popleft()

        for i in range(4):
            nx = x+dx[i]
            ny = y+dy[i]

            if 0<=nx<N and 0<=ny<M and graph[nx][ny] == 0:
                q.append((nx,ny))
                graph[nx][ny] = graph[x][y] + 1

lst_answer = []
for i in range(N):
    for j in range(M):
        if graph[i][j] == 1:
            temp = copy.deepcopy(graph)
            temp[i][j] = 0
            bfs(0,0,temp)
            
            if temp[N-1][M-1] == 0:
                continue
            
            lst_answer.append(temp[N-1][M-1]+1)

if not lst_answer:
    print(-1)
else:
    print(min(lst_answer))
```

## TRY 3

```python
import sys
from collections import deque
input = sys.stdin.readline
N,M = map(int,input().split())

graph = [[0]*M for _ in range(N)]
# visited[x][y][0] --> 벽 파괴 기회 O
# visited[x][y][1] --> 벽 파괴 기회 X
visited = [[[0]*2 for _ in range(M)] for _ in range(N)]

# 맵 생성
for i in range(N):
    line = input()
    for j in range(M):
        graph[i][j] = int(line[j])


dx = [1,-1,0,0]
dy = [0,0,1,-1]

# BFS로 최단거리 탐색
def bfs(a,b,c):
    visited[a][b][c] = 1
    q = deque([(a,b,c)])

    while q:
        x,y,z = q.popleft()

        # 끝 지점 도달
        if x == N-1 and y == M-1:
            return visited[x][y][z]

        # 상하좌우 탐색 해봄
        for i in range(4):
            nx = x+dx[i]
            ny = y+dy[i]

            if 0<=nx<N and 0<=ny<M and visited[nx][ny][z] == 0:
                # Case1. 벽 & 벽 파괴 기회 O
                # --> (nx,ny) 벽 파괴 기회 삭제 & 큐에 추가(이동)
                if graph[nx][ny] == 1 and z == 0:
                    visited[nx][ny][1] = visited[x][y][0] + 1
                    q.append((nx,ny,1))

                # Case2. 벽 & 벽 파괴 기회 X
                # --> 이동 불가

                # Case3. 길
                # --> 이동
                if graph[nx][ny] == 0:
                    visited[nx][ny][z] = visited[x][y][z] + 1
                    q.append((nx,ny,z))
    
    return -1

print(bfs(0,0,0))
```
