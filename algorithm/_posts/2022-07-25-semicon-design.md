---
layout: post
title: "[백준 - 2352] 반도체 설계"
description: >
  백준 Gold2
sitemap: false
tags: [LIS,lower bound]
hide_last_modified: true
---

# 📱 반도체 설계

[반도체 설계](https://www.acmicpc.net/problem/2352)

## 문제

반도체를 설계할 때 n개의 포트를 다른 n개의 포트와 연결해야 할 때가 있다.

<p align="center">
<img style="margin:50px 0 10px 0" src="https://www.acmicpc.net/JudgeOnline/upload/201103/chip.png" alt/>
</p> 

예를 들어 왼쪽 그림이 n개의 포트와 다른 n개의 포트를 어떻게 연결해야 하는지를 나타낸다. 하지만 이와 같이 연결을 할 경우에는 연결선이 서로 꼬이기 때문에 이와 같이 연결할 수 없다. n개의 포트가 다른 n개의 포트와 어떻게 연결되어야 하는지가 주어졌을 때, 연결선이 서로 꼬이지(겹치지, 교차하지) 않도록 하면서 최대 몇 개까지 연결할 수 있는지를 알아내는 프로그램을 작성하시오

## 입력

첫째 줄에 정수 n(1 ≤ n ≤ 40,000)이 주어진다. 다음 줄에는 차례로 1번 포트와 연결되어야 하는 포트 번호, 2번 포트와 연결되어야 하는 포트 번호, …, n번 포트와 연결되어야 하는 포트 번호가 주어진다. 이 수들은 1 이상 n 이하이며 서로 같은 수는 없다고 가정하자.

## 출력

첫째 줄에 최대 연결 개수를 출력한다.

## 예제 입력 1 

```
6
4 2 6 3 1 5
```

## 예제 출력 1 

```
3
```

# 📖 알고리즘

## LIS 풀이법 1: DP

처음 이 문제를 보고 DP로 바로 접근하였다. 
[전깃줄](https://www.acmicpc.net/problem/2565) 문제, [LIS](https://www.acmicpc.net/problem/11053) 문제와 비슷하다고 느꼈기 때문이다.

line[i]를 i번째 원소를 마지막으로 하는 LIS 길이라고 정의하여 풀면 되는데 시간 초과가 발생하였다.

```python

import sys

input = sys.stdin.readline

n = int(input())
port = list(map(int,input().split()))

# line[i]: i번 포트에서 최대로 연결할 수 있는 선의 개수
line = [1]*(n)

for i in range(n) :
    for j in range(i) :
        if port[i] > port[j] :
            line[i] = max(line[i],line[j]+1)

print(line[n-1])
```

## LIS 풀이법 2: Lower Bound

결국 솔루션을 참조했고 더욱 적은 시간복잡도로 풀 수 있는 방법을 배웠다. 

👉🏻 **Lower Bound는 숫자를 [정렬된 배열에 삽입시 제일 작은 index를 얻고자 할 때] 사용한다.**

이것을 왜 사용하는지는 아래 진행과정을 살펴보면 된다.

![연습장-3](https://user-images.githubusercontent.com/88064555/180662832-c3937269-5221-479a-9921-1aefc6ce4f61.jpg)

<img width="416" alt="스크린샷 2022-07-25 오전 4 32 41" src="https://user-images.githubusercontent.com/88064555/180662933-bfe98550-b9e9-4e5d-b2ed-de1792bab999.png">

어떤 숫자 n을 넣는다고 가정해보자. 만일 LIS 배열이 비어있거나 마지막 원소가 n보다 작다면 n을 LIS 배열에 추가시킨다.

그런데 LIS 배열의 마지막 원소가 n과 같거나 더 크다면 n을 추가 시켜주는데 이 때, LIS는 오름차순 정렬이기 때문에

Lower Bound를 이용하여 삽입할 index를 찾아준다.

# 👨🏻‍💻 CODE

```python
import sys
input = sys.stdin.readline

# 초기 조건
n = int(input())
port = list(map(int,input().split()))

# 정렬된 배열에서 value가 위치할 수 있는 가장 작은 index
def lower_bound(start,end,value,arr):
    while start < end:
        mid = (start+end)//2

        if arr[mid] < value:
            start = mid+1
        else:
            end = mid
    
    return end

lis = []

for i in range(n):
    if i == 0:
        lis.append(port[i])
    else:
        if lis[-1] < port[i]:
            lis.append(port[i])
        else:
            idx = lower_bound(0,len(lis),port[i],lis)
            lis[idx] = port[i]

print(len(lis))
```
