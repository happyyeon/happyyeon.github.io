---
layout: post
title: "[백준 - 7795] 먹을 것인가 먹힐 것인가"
description: >
  백준 Silver3
sitemap: false
tags: [Binary Search, Two Pointer]
hide_last_modified: true
---
# 🐊 먹을 것인가 먹힐 것인가

[먹을 것인가 먹힐 것인가](https://www.acmicpc.net/problem/7795)

## 문제
---
심해에는 두 종류의 생명체 A와 B가 존재한다. A는 B를 먹는다. A는 자기보다 크기가 작은 먹이만 먹을 수 있다. 예를 들어, A의 크기가 {8, 1, 7, 3, 1}이고, B의 크기가 {3, 6, 1}인 경우에 A가 B를 먹을 수 있는 쌍의 개수는 7가지가 있다. 8-3, 8-6, 8-1, 7-3, 7-6, 7-1, 3-1.

![img](https://www.acmicpc.net/upload/images/ee(1).png)

두 생명체 A와 B의 크기가 주어졌을 때, A의 크기가 B보다 큰 쌍이 몇 개나 있는지 구하는 프로그램을 작성하시오.

## 입력
---
첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스의 첫째 줄에는 A의 수 N과 B의 수 M이 주어진다. 둘째 줄에는 A의 크기가 모두 주어지며, 셋째 줄에는 B의 크기가 모두 주어진다. 크기는 양의 정수이다. (1 ≤ N, M ≤ 20,000)

## 출력
---
각 테스트 케이스마다, A가 B보다 큰 쌍의 개수를 출력한다.


## 예제 입력 1 

```
2
5 3
8 1 7 3 1
3 6 1
3 4
2 13 7
103 11 290 215
```

## 예제 출력 1 

```
7
1
```

# 📖 가져다 쓰기

문제 입력 개수가 2만이다. 브루트 포스로 안 풀린다. 어떤 알고리즘을 써야 할지 감이 안 왔다.

그래서 그냥 센스껏 풀어보았다. 일단 코드의 시간복잡도를 O(n)으로 맞추는데 집중했다.

# 📐 과정 설계/관리

![노트북-10](https://user-images.githubusercontent.com/88064555/182356627-d4d5c30c-f199-4baa-94b5-c71302e6a353.jpg)

입력으로 들어오는 포식자 배열과 먹히는 배열을 한 배열로 몰아 담는다.

이 때, 튜플 형식으로 flag를 준다.

이를 테면, (1,e)는 포식자이며 크기가 1이라는 뜻이고 (6,f)는 먹이이며 크기가 6이라는 뜻이다.

그리고 그 배열을 정렬한다. 일단 크기 순으로 정렬될 것이며 크기가 같다면 포식자가 우선순위로 정렬될 것이다.

그렇게 하여 전체 배열을 순회하며 포식자가 나오면 그 앞에 있는 먹이들을 다 먹어치울 것이고 먹이가 나오면 먹이 개수를 증가시켜준다.

# 👨🏻‍💻 CODE

```python
import sys
input = sys.stdin.readline

def solution():
    answer = 0
    b_count = 0

    arr.sort()

    for i in range(len(arr)):
        if arr[i][1] == 'e':
            answer += b_count
        if arr[i][1] == 'f':
            b_count += 1
    
    return answer
    

for _ in range(int(input())):
    n,m = map(int,input().split())
    arr = []
    temp = list(map(int,input().split()))
    for i in range(n):
        arr.append((temp[i],'e'))
    temp = list(map(int,input().split()))
    for i in range(m):
        arr.append((temp[i],'f'))
    

    answer = solution()
    print(answer)
```

# 다른 사람의 풀이

문제를 다 푼후, 알고리즘 분류를 보았는데 투포인터와 이진탐색으로 풀 수 있었다. 

**2개의 정수 배열을 비교할 때 투포인터 혹은 이진탐색**을 사용하면 더욱 빠른 시간으로 문제를 풀 수 있다는 사실을 깨달았다.

## bisect library

Python에서는 "정렬된 배열"에서 특정한 원소를 찾을 때 아주 효과적으로 사용할 수 있게 해주는 함수가 존재한다.

바로 bisect 함수이다.

- bisect_left(a, x) : 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 왼쪽 인덱스를 반환

- bisect_right(a, x) : 정렬된 순서를 유지하면서 리스트 a에 데이터 x를 삽입할 가장 오른쪽 인덱스를 반환

![img](https://velog.velcdn.com/images%2Fssongplay%2Fpost%2F5ee207f1-56ae-43fa-9584-a77cc069c6fd%2Fimage.png)

```python
import bisect

for _ in range(int(input())):
    N, M = map(int, input().split())
    A = sorted(list(map(int, input().split())))
    B = sorted(list(map(int, input().split())))
    cnt = 0
    for a in A:
        cnt += (bisect.bisect(B, a-1))
    print(cnt)
```

