---
layout: post
title: "[백준 - 14938] 서강그라운드"
description: >
  백준 Class4 Gold4
sitemap: false
tags: [Floyd Warshall]
hide_last_modified: true
---

# 🎓 서강그라운드

solved_ac[Class4] [서강그라운드](https://www.acmicpc.net/problem/14938)

## 문제
---
예은이는 요즘 가장 인기가 있는 게임 서강그라운드를 즐기고 있다. 서강그라운드는 여러 지역중 하나의 지역에 낙하산을 타고 낙하하여, 그 지역에 떨어져 있는 아이템들을 이용해 서바이벌을 하는 게임이다. 서강그라운드에서 1등을 하면 보상으로 치킨을 주는데, 예은이는 단 한번도 치킨을 먹을 수가 없었다. 자신이 치킨을 못 먹는 이유는 실력 때문이 아니라 아이템 운이 없어서라고 생각한 예은이는 낙하산에서 떨어질 때 각 지역에 아이템 들이 몇 개 있는지 알려주는 프로그램을 개발을 하였지만 어디로 낙하해야 자신의 수색 범위 내에서 가장 많은 아이템을 얻을 수 있는지 알 수 없었다.

각 지역은 일정한 길이 l (1 ≤ l ≤ 15)의 길로 다른 지역과 연결되어 있고 이 길은 양방향 통행이 가능하다. 예은이는 낙하한 지역을 중심으로 거리가 수색 범위 m (1 ≤ m ≤ 15) 이내의 모든 지역의 아이템을 습득 가능하다고 할 때, 예은이가 얻을 수 있는 아이템의 최대 개수를 알려주자.

![img](https://upload.acmicpc.net/ef3a5124-833a-42ef-a092-fd658bc8e662/-/preview/)

주어진 필드가 위의 그림과 같고, 예은이의 수색범위가 4라고 하자. ( 원 밖의 숫자는 지역 번호, 안의 숫자는 아이템 수, 선 위의 숫자는 거리를 의미한다) 예은이가 2번 지역에 떨어지게 되면 1번,2번(자기 지역), 3번, 5번 지역에 도달할 수 있다. (4번 지역의 경우 가는 거리가 3 + 5 = 8 > 4(수색범위) 이므로 4번 지역의 아이템을 얻을 수 없다.) 이렇게 되면 예은이는 23개의 아이템을 얻을 수 있고, 이는 위의 필드에서 예은이가 얻을 수 있는 아이템의 최대 개수이다.

## 입력
---
첫째 줄에는 지역의 개수 n (1 ≤ n ≤ 100)과 예은이의 수색범위 m (1 ≤ m ≤ 15), 길의 개수 r (1 ≤ r ≤ 100)이 주어진다.

둘째 줄에는 n개의 숫자가 차례대로  각 구역에 있는 아이템의 수 t (1 ≤ t ≤ 30)를 알려준다.

세 번째 줄부터 r+2번째 줄 까지 길 양 끝에 존재하는 지역의 번호 a, b, 그리고 길의 길이 l (1 ≤ l ≤ 15)가 주어진다.

## 출력
---
예은이가 얻을 수 있는 최대 아이템 개수를 출력한다.

## 예제 입력 1 

```
5 5 4
5 7 8 2 3
1 4 5
5 2 4
3 2 3
1 2 3
```

## 예제 출력 1 

```
23
```

# 📖 알고리즘

이 문제의 핵심을 간파하면 다음과 같다.

+ Weight가 존재하는 그래프이며 문제 조건에 제한이 있다.
+ 각 노드에 Value가 존재한다.
+ 모든 노드에서 출발하여 모든 노드로 도착하는 가장 빠른 경로를 구해야 한다.

여기서 제일 중요한 부분은 마지막 조건이다. 3번을 Base로 깔아두고 1번과 2번 조건을 얹어야 한다. 

그렇다면 3번 조건에 부합하는 알고리즘은 무엇일까? [플로이드](https://www.acmicpc.net/problem/11404) 문제에서 우리는 다음과 같은 지식을 얻었다.

**[플로이드 와샬 알고리즘은 모든 노드에서 모든 노드로 가는 최소 비용을 구할 때 사용한다.]**

혹여, 3번 조건이 왜 필요한지 이해가 안 가시는 분들을 위하여 설명을 하자면 예은이가 수색할 수 있는 범위는 한정적이다.

따라서 최대한 많은 아이템을 줍기 위해서는 각 지역을 최단 경로로 탐색해야 제한된 수색 범위 내에서 최대한 이득을 볼 수 있기 떄문이다.

# 📐 설계

![연습장-35](https://user-images.githubusercontent.com/88064555/178984222-96d9dd28-c2c0-4280-97bf-42d382576b12.jpg)

1. 플로이드 와샬 ➡️ 모든 노드에서 모든 노드로 가는 최소 비용 테이블
2. 각 노드에서 출발하였을 때, [최소 비용 ≤ 수색 범위] 
3. 2번 조건에 부합하는 item 개수 저장
4. 3번 조건에서 제일 큰 item 개수 출력

# 👨🏻‍💻 CODE

```python
INF = int(1e9)
region, search, road = map(int,input().split())

item = list(map(int,input().split()))

graph = [[INF]*region for _ in range(region)]

for i in range(road):
    start,end,distance = map(int,input().split())

    graph[start-1][end-1] = distance
    graph[end-1][start-1] = distance

for i in range(region):
    graph[i][i] = 0

# 플로이드 와샬 알고리즘
for i in range(region):
    for j in range(region):
        for k in range(region):
            if graph[j][k] > graph[j][i] + graph[i][k]:
                graph[j][k] = graph[j][i] + graph[i][k]


lst_answer = []
for i in range(region):
    count = 0
    for j in range(region):
        if graph[i][j] <= search:
            count += item[j]
    lst_answer.append(count)

print(max(lst_answer))
```


