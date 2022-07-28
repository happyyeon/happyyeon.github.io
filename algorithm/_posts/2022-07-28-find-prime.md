---
layout: post
title: "[프로그래머스] 소수 찾기"
description: >
  프로그래머스 Level2
sitemap: false
tags: [Brute Force, 에라토스테네스의 체]
hide_last_modified: true
---

# 🔎 소수 찾기

# 📖 가져다 쓰기

문제 분류에 "완전탐색"이라고 쓰여 있어서 알고리즘을 이미 알고 들어갔다. 

추가적인 조건이라면 소수를 찾는 문제이기에 **에라토스테네스의 체** 를 쓰면 되겠다고 생각하였다.

# 📐 과정 설계/관리

![연습장-59](https://user-images.githubusercontent.com/88064555/181423333-e85ac4fc-45ad-4835-ba71-19b5dc3a1d52.jpg)

로직을 짜다보니 순열이 필요하였다. 파이썬 내장 모듈인 permutation을 import하여 사용하였다.

# 👨🏻‍💻 CODE

```python
from itertools import permutations as p

MAX = 9999999

# 소수 검증 함수 - 에라토스테네스의 체
def is_prime(n):
    sieve = [True]*(n+1)

    m = int(n**0.5)

    for i in range(2,m+1):
        if sieve[i] == True:
            for j in range(2*i,n+1,i):
                sieve[j] = False

    return sieve

prime = is_prime(MAX)
prime[0], prime[1] = False,False

def solution(numbers):
    arr = list(map(int,numbers))
    permu = []
    for i in range(1,len(arr)+1):
        permu.extend(list(p(arr,i)))
    result = []
    for i in range(len(permu)):
        temp = ""
        for j in range(len(permu[i])):
            temp += str(permu[i][j])
        result.append(int(temp))
    result = set(result)
    result = list(result)

    count = 0
    for i in range(len(result)):
        if prime[result[i]] == True:
            count += 1

    return count
```

## 고수의 풀이

이 분은 진정한 파이토닉인듯 ...

```python
from itertools import permutations
def solution(n):
    a = set()
    for i in range(len(n)):
        a |= set(map(int, map("".join, permutations(list(n), i + 1))))
    a -= set(range(0, 2))
    for i in range(2, int(max(a) ** 0.5) + 1):
        a -= set(range(i * 2, max(a) + 1, i))
    return len(a)
```
