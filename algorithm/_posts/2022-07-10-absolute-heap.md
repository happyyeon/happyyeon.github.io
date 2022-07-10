---
layout: post
title: "[백준 - 11286] 절댓값 힙"
description: >
  백준 Class3 Silver1
sitemap: false
tags: [Heap]
hide_last_modified: true
---

# 절댓값 힙

solved_ac[Class3] [절댓값 힙](https://www.acmicpc.net/problem/11286)

## 문제
절댓값 힙은 다음과 같은 연산을 지원하는 자료구조이다.

    1.배열에 정수 x (x ≠ 0)를 넣는다.
    2.배열에서 절댓값이 가장 작은 값을 출력하고, 그 값을 배열에서 제거한다. 절댓값이 가장 작은 값이 여러개일 때는, 가장 작은 수를 출력하고, 그 값을 배열에서 제거한다.
프로그램은 처음에 비어있는 배열에서 시작하게 된다.

## 입력

첫째 줄에 연산의 개수 N(1≤N≤100,000)이 주어진다. 다음 N개의 줄에는 연산에 대한 정보를 나타내는 정수 x가 주어진다. 만약 x가 0이 아니라면 배열에 x라는 값을 넣는(추가하는) 연산이고, x가 0이라면 배열에서 절댓값이 가장 작은 값을 출력하고 그 값을 배열에서 제거하는 경우이다. 입력되는 정수는 -231보다 크고, 231보다 작다.

## 출력

입력에서 0이 주어진 회수만큼 답을 출력한다. 만약 배열이 비어 있는 경우인데 절댓값이 가장 작은 값을 출력하라고 한 경우에는 0을 출력하면 된다.

## 예제 입력 1 

```
18
1
-1
0
0
0
1
1
-1
-1
2
-2
0
0
0
0
0
0
0
```

## 예제 출력 1 

```
-1
1
0
-1
-1
1
1
-2
2
0
```

# 🛠 자료구조

Python의 최소 힙 특징을 이용하는 문제이다. 어떻게 응용하였는지는 아래에서 설명하도록 하겠다.

# 📐 과정설계

![연습장-10](https://user-images.githubusercontent.com/88064555/178125574-2af83b83-14af-46ac-8bd8-0cab1c5aa034.jpg)

Python의 heap 모듈은 최소 힙이다. 여기서 포인트는 최소 힙 정렬이 배열에도 적용된다는 점이다.

따라서 heappush를 할 때, 일단 절댓값 기준으로 push를 하되 부호도 같이 붙여준다.

그리고 출력할 때, 부호가 +면 그대로 -면 음수로 출력해주면 된다.

이 때 드는 의문!! 그림을 참조해보았을 때, -3과 3을 비교하여 -3를 먼저 출력해주도록 해야하는데 이는 어떻게 할 것인가?

최소 힙이라는 관점에서 봤을 때 이는 문제가 되지 않는다. 왜냐하면 index 0번을 비교해보았을 때 -3과 3은 같은 절댓값 3이다. 

그랬을 때 python heap은 그 다음 차례인 index 1번을 기준으로 다시 비교를 한다. -를 Flase(0) +를 True(1)로 설정하여 넣어준다면 False < True이므로 

자연스럽게 -3이 먼저 출력이 된다. 

# 👨🏻‍💻 CODE

```python
import sys
import heapq as h
input = sys.stdin.readline

N = int(input())

heap = []

for _ in range(N):
    num = int(input())

    if num > 0:
        h.heappush(heap,[num,True])
    
    if num < 0:
        h.heappush(heap,[-1*num,False])

    if num == 0:
        if not heap:
            print(0)
        else:
            element = h.heappop(heap)

            answer, flag = element[0], element[1]

            if flag == True:
                print(answer)
            else:
                print(-1*answer)
```
