---
layout: post
title:  "[백준 2407] - 조합"
tags: [재귀,Dynamic Programming]
hide_last_modified: true
---
# 조합

solved_ac[Class4] [조합](https://www.acmicpc.net/problem/2407)

## 문제
nCm을 출력한다.

## 입력
n과 m이 주어진다. (5 ≤ n ≤ 100, 5 ≤ m ≤ 100, m ≤ n)

## 출력
nCm을 출력한다.
## 예제 입력 1 

```
100 6
```

## 예제 출력 1 

```
1192052400
```

# 어떤 알고리즘을 쓰는가
**[재귀]**

## 1트

```python
from itertools import combinations
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

# 초기 조건
n,m = map(int,input().split())

arr = [i for i in range(n)]
print(len(list(combinations(arr,m))))
```
combinaion 모듈을 사용하여 날먹좀 하려 했지만 역시나 시간초과

## 2트, 재귀 (76ms)

```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

# 초기 조건
n,m = map(int,input().split())

# n!
def fact(n):
    if n == 1:
        return 1
    else:
        return n*fact(n-1)

answer = fact(n)//(fact(n-m)*fact(m))
print(answer)
```

[nCm = n!/(n-m)!m!]이다. 팩토리얼 계산을 재귀를 통하여 구하였다. 

왜 1트보다 2트가 빠른 것일까?

fact 함수를 살펴보면 O(N)이라고 생각한다. 왜냐하면 n=100인 경우에 fact(99)부터 fact(1)까지 함수 실행을 해야하기 때문이다.

그렇다면 Combination 모듈은 시간복잡도가 O(N) 이상인건가? 구글링을 해보았는데 잘 모르겠다.

## 3트, DP (68ms)

```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

# 초기 조건
n,m = map(int,input().split())

arr_fact = [1]*101

# n!
for i in range(2,101):
    arr_fact[i] = i*arr_fact[i-1]

answer = arr_fact[n]//(arr_fact[n-m]*arr_fact[m])
print(answer)
```

정답을 맞춘 후 알고리즘 분류를 확인해보니 DP라고 되어있었다. 그래서 memoization 기법으로도 풀어보았는데 2트랑 비교해보았을 때 시간차이가 얼마 나지 않았다.

그 이유를 생각해본다면, dp 배열을 생성할 때도 어차피 n번만큼 생성을 해주어야 하기 때문에 

똑같은 O(N)의 시간복잡도를 가지기 때문이라고 추측해본다.







