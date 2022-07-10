---
layout: post
title: "[백준 - 2667] 단지번호붙이기"
description: >
  백준 Class3 Silver1
sitemap: false
tags: [BFS]
hide_last_modified: true
---

# 🏠 단지번호붙이기

solved_ac[Class3] [단지번호붙이기](https://www.acmicpc.net/problem/2667)

## 문제
<그림 1>과 같이 정사각형 모양의 지도가 있다. 1은 집이 있는 곳을, 0은 집이 없는 곳을 나타낸다. 철수는 이 지도를 가지고 연결된 집의 모임인 단지를 정의하고, 단지에 번호를 붙이려 한다. 여기서 연결되었다는 것은 어떤 집이 좌우, 혹은 아래위로 다른 집이 있는 경우를 말한다. 대각선상에 집이 있는 경우는 연결된 것이 아니다. <그림 2>는 <그림 1>을 단지별로 번호를 붙인 것이다. 지도를 입력하여 단지수를 출력하고, 각 단지에 속하는 집의 수를 오름차순으로 정렬하여 출력하는 프로그램을 작성하시오.

![img](https://www.acmicpc.net/upload/images/ITVH9w1Gf6eCRdThfkegBUSOKd.png)



## 입력
첫 번째 줄에는 지도의 크기 N(정사각형이므로 가로와 세로의 크기는 같으며 5≤N≤25)이 입력되고, 그 다음 N줄에는 각각 N개의 자료(0혹은 1)가 입력된다.

## 출력
첫 번째 줄에는 총 단지수를 출력하시오. 그리고 각 단지내 집의 수를 오름차순으로 정렬하여 한 줄에 하나씩 출력하시오.

## 예제 입력 1 

```
7
0110100
0110101
1110101
0000111
0100000
0111110
0111000
```

## 예제 출력 1 

```
3
7
8
9
```

# 🛠 자료구조

BFS를 사용하여 풀었다. 특정 1 노드를 기점으로 하여 더 이상 1이 안 나올때까지 완전탐색을 하는 것이기 때문에 DFS,BFS 무엇을 선택하든 상관 없었다.

나는 visited 배열을 따로 생성하지 않고 그래프 자체 내에서 해결하였다. 

![연습장-5](https://user-images.githubusercontent.com/88064555/178078917-ccca0ad9-3069-4f28-a05e-6c0ce50e45ac.jpg)

마지막 오름차순 정렬까지 고려하여 집 9개부터 탐색한다고 가정하였다.

그래프의 처음부터 끝까지 1인 노드들을 탐색하며 2로 바꿔준다. 그리고 BFS탐색이 종료되었으면 최종 집의 개수를 정답 배열에 추가한다.

그렇게 해서 나온 정답 배열을 오름차순 정렬을 시킨 후, 정답을 형식에 맞게 출력한다.

# 👨🏻‍💻 CODE

```python
from collections import deque

n = int(input())
graph = [[0]*n for _ in range(n)]
dx = [1,-1,0,0]
dy = [0,0,1,-1]

for i in range(n):
    house_info = input()
    for j in range(n):
        graph[i][j] = int(house_info[j])

def bfs(x,y):
    house_count = 0
    q = deque([[x,y]])
    graph[x][y] = 2
    while q:
        cur_node = q.popleft()
        house_count += 1

        for i in range(4):
            new_x = cur_node[0]+dx[i]
            new_y = cur_node[1]+dy[i]

            if 0<=new_x<n and 0<=new_y<n:
                if graph[new_x][new_y] == 1:
                    q.append([new_x,new_y])
                    graph[new_x][new_y] = 2
    
    return house_count

lst_answer = []

for x in range(n):
    for y in range(n):
        if graph[x][y] == 1:
            lst_answer.append(bfs(x,y))
lst_answer.sort()
print(len(lst_answer))
for answer in lst_answer:
    print(answer)
```



