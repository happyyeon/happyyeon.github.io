---
layout: post
title: "[백준 - 17779] 게리 맨더링 2"
description: >
  백준 Gold4
sitemap: false
tags: [Brute Force]
hide_last_modified: true
---

# 🗳 게리 맨더링 2

# 📖 알고리즘

👉🏻 **브루트 포스 알고리즘은 [문제 제한 조건이 작을 때] 써볼까? 할 수 있다.**

사실 "작다"라는 개념은 상대적이기에 가늠하기 어려울 수도 있지만 이 문제의 조건을 봐보자.

+ 5 ≤ N ≤ 20
+ 1 ≤ A[r][c] ≤ 100

딱 봐도 작다는 Feel이 온다 ... 때로는 단순무식한 방법이 통한다.

# 📐 설계

![연습장-62](https://user-images.githubusercontent.com/88064555/180012854-904e1420-f55c-4b3d-b178-8c53dd477ea1.jpg)

모든 x,y,d1,d2에 대하여 위 플로우 차트를 수행한다.


## 잘못된 설계

<img width="744" alt="스크린샷 2022-07-21 오후 5 18 04" src="https://user-images.githubusercontent.com/88064555/180165514-5a3c4611-f170-4552-84a8-f9099c839a31.png">

Brute Force를 수행하면서 x,y,d1,d2의 조건을 걸어주어야 한다. 그러나 조건을 자세히 보면 최초 출발지가 (1,1)이다.

Python에서는 리스트의 인덱스가 0부터 시작하기 때문에 컴퓨터는 (0,0)이 시작이다. 따라서 나는 위의 조건 전부에 -1 처리를 해주었다.

그런데 range 범위를 잘못 설정해준 것인지 계속 오답이 출력되었다. 그래서 문제 조건은 그대로 가져오되 

기존 city 2차원 배열에서 인구수를 가져올 때 (x,y) ➡️ (x-1,y-1) 처리를 하여 가져왔다. 

# 👨🏻‍💻 CODE

```python
import sys
input = sys.stdin.readline
INF = int(1e9)
# Initialize
N = int(input())
city = []
for i in range(N):
    city.append(list(map(int,input().split())))

total = 0 # 총원
for i in range(N):
    for j in range(N):
        total += city[i][j]

def sol(x,y,d1,d2):
    temp = [[0]*N for _ in range(N)]
    # 선거구 5 세팅
    for i in range(d1+1):
        temp[x+i-1][y-i-1] = 5
        temp[x+d2+i-1][y+d2-i-1] = 5
    for i in range(d2+1):
        temp[x+i-1][y+i-1] = 5
        temp[x+d1+i-1][y-d1+i-1] = 5


    max_people = -1*INF
    min_people = INF
    
    sum1 = 0
    # 선거구 1 세팅
    for i in range(1,x+d1):
        for j in range(1,y+1):
            if temp[i-1][j-1] == 5:
                break
            # 선거구 1 인원
            sum1 += city[i-1][j-1]
    
    # Max Min Update
    if max_people < sum1:
        max_people = sum1

    if min_people > sum1:
        min_people = sum1
    
    sum2 = 0
    # 선거구 2 세팅 (열의 개수가 점점 작아지므로 역탐색 해야함)
    for i in range(1,x+d2+1):
        for j in range(N,y,-1):
            if temp[i-1][j-1] == 5:
                break

            # 선거구 2 인원
            sum2 += city[i-1][j-1]
    
    # Max Min Update
    if max_people < sum2:
        max_people = sum2

    if min_people > sum2:
        min_people = sum2
    
    # 선거구 3 세팅
    sum3 = 0
    for i in range(x+d1,N+1):
        for j in range(1,y-d1+d2):
            if temp[i-1][j-1] == 5:
                break
            # 선거구 3 인원
            sum3 += city[i-1][j-1]
    
    # Max Min Update
    if max_people < sum3:
        max_people = sum3

    if min_people > sum3:
        min_people = sum3
    
    sum4 = 0
    # 선거구 4 세팅 (열의 개수가 점점 작아지므로 역탐색 해야함)
    for i in range(x+d2+1,N+1):
        for j in range(N,y-d1+d2-1,-1):
            if temp[i-1][j-1] == 5:
                break

            # 선거구 4 인원
            sum4 += city[i-1][j-1]
    
    # Max Min Update
    if max_people < sum4:
        max_people = sum4

    if min_people > sum4:
        min_people = sum4
    
    # 선거구 5 인원
    sum5 = total - sum1 - sum2 - sum3 - sum4

    # Max Min Update
    if max_people < sum5:
        max_people = sum5

    if min_people > sum5:
        min_people = sum5
    
    return max_people-min_people

answer = INF

for d1 in range(1,N):
    for d2 in range(1,N):
        for x in range(1,N-d1-d2+1):
            for y in range(d1+1,N-d2+1):
                answer = min(answer,sol(x,y,d1,d2))

                

print(answer)
```






