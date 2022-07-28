---
layout: post
title: "[백준 - 6588] 골드바흐의 추측"
description: >
  백준 Silver1
sitemap: false
tags: [에라토스테네스의 체]
hide_last_modified: true
---

# 🕵🏻‍♂️ 골드바흐의 추측

# 📖 알고리즘

👉🏻 **에라토스테네스의 체는 [소수 검증]을 할 때 사용한다.**

이 문제의 핵심은 특정 숫자가 소수인지 아닌지 판별을 해야한다는 것이다.

n = a+b일 때, b-a가 최대가 되도록 설정을 해야하므로 2부터 n-1까지 소수 판별을 해야한다.

이해가 쉽도록 예를 들어보겠다.

# 📐 설계

![연습장-67](https://user-images.githubusercontent.com/88064555/180207997-316438ab-844e-4352-8d71-d73c2e8139bb.jpg)

b-a가 최대로 되도록 하려면 a를 2부터 시작해서 골드바흐의 추측을 만족시킬때 STOP을 하면 된다.

범위가 2~n-1인 이유는 1은 어차피 소수가 아니기 때문이다.

## 시간 초과 설계

처음에는 n이 들어올 때마다 소수 판별 함수를 가동시켜 답을 출력하도록 하였는데 그렇게 하면 시간 초과가 발생한다.

그보다는 입력 제한 범위가 이미 정해져있으니 미리 백만까지의 소수판별 배열을 생성한 다음 골드바흐의 추측을 진행하는 것이 더욱 효율적이다.

# 👨🏻‍💻 CODE

## TLE

```python
import sys
input = sys.stdin.readline

# 소수 검증 함수 - 에라토스테네스의 체
def is_prime(n):
    sieve = [True]*(n+1)

    m = int(n**0.5)

    for i in range(2,m+1):
        if sieve[i] == True:
            for j in range(2*i,n+1,i):
                sieve[j] = False

    return sieve

while True:
    n = int(input())

    if n == 0:
        break
    
    sieve = is_prime(n)
    for i in range(2,n-1):
        if sieve[i] == True and sieve[n-i] == True:
            print("{0} = {1} + {2}".format(n,i,n-i))
            break
```

## 정답

```python
import sys
input = sys.stdin.readline

# 소수 검증 함수 - 에라토스테네스의 체
def is_prime(n):
    sieve = [True]*(n+1)

    m = int(n**0.5)

    for i in range(2,m+1):
        if sieve[i] == True:
            for j in range(2*i,n+1,i):
                sieve[j] = False

    return sieve

sieve = is_prime(1000000)

while True:
    n = int(input())

    if n == 0:
        break
    
    for i in range(2,n-1):
        if sieve[i] == True and sieve[n-i] == True:
            print("{0} = {1} + {2}".format(n,i,n-i))
            break
```


