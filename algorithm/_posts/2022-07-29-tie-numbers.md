---
layout: post
title: "[백준 - 1744] 수 묶기"
description: >
  백준 Gold4
sitemap: false
tags: [Greedy]
hide_last_modified: true
---
# 🪢 수 묶기

[수 묶기](https://www.acmicpc.net/problem/1744)

## 문제
---
길이가 N인 수열이 주어졌을 때, 그 수열의 합을 구하려고 한다. 하지만, 그냥 그 수열의 합을 모두 더해서 구하는 것이 아니라, 수열의 두 수를 묶으려고 한다. 어떤 수를 묶으려고 할 때, 위치에 상관없이 묶을 수 있다. 하지만, 같은 위치에 있는 수(자기 자신)를 묶는 것은 불가능하다. 그리고 어떤 수를 묶게 되면, 수열의 합을 구할 때 묶은 수는 서로 곱한 후에 더한다.

예를 들면, 어떤 수열이 {0, 1, 2, 4, 3, 5}일 때, 그냥 이 수열의 합을 구하면 0+1+2+4+3+5 = 15이다. 하지만, 2와 3을 묶고, 4와 5를 묶게 되면, 0+1+(2*3)+(4*5) = 27이 되어 최대가 된다.

수열의 모든 수는 단 한번만 묶거나, 아니면 묶지 않아야한다.

수열이 주어졌을 때, 수열의 각 수를 적절히 묶었을 때, 그 합이 최대가 되게 하는 프로그램을 작성하시오.

## 입력
---
첫째 줄에 수열의 크기 N이 주어진다. N은 50보다 작은 자연수이다. 둘째 줄부터 N개의 줄에 수열의 각 수가 주어진다. 수열의 수는 -1,000보다 크거나 같고, 1,000보다 작거나 같은 정수이다.

## 출력
---
수를 합이 최대가 나오게 묶었을 때 합을 출력한다. 정답은 항상 231보다 작다.

## 예제 입력 1 

```
4
-1
2
1
3
```

## 예제 출력 1 

```
6
```

## 예제 입력 2

```
6
0
1
2
4
3
5
```

## 예제 출력 2 

```
27
```

## 예제 입력 3

```
1
-1
```

## 예제 출력 3

```
-1
```

## 예제 입력 4

```
3
-1
0
1
```

## 예제 출력 4

```
1
```

## 예제 입력 5

```
2
1
1
```

## 예제 출력 5

```
2
```

# 📖 가져다 쓰기

## 브루트 포스(실패)

처음에는 입력 제한 조건을 보고 브루트 포스로 풀어도 되겠다고 생각하였다. 그러나 생각보다 구현 방식이 너무 어려웠고 설계의 가닥이 안 잡혔다.

## 그리디

그래서 알고리즘 분류를 확인해보았을 때 그리디라는 것을 보고 이 문제에서 왜 그리디를 사용할까를 고찰해보았다.

2개의 수를 묶어서 최대를 맞추어야 하는 것인데 그 두개를 뽑을 때 마다 최대한의 이득을 보면 최종적인 최대를 구할 수 있다.

그래서 어떻게 하면 최대한의 이득을 볼 수 있을까를 생각해 본 결과가 아래와 같다.

# 📐 과정 설계/관리

![연습장-69](https://user-images.githubusercontent.com/88064555/181755808-00e86fba-22c1-4d2d-9d3c-aaf9785b7bea.jpg)

+ 양수를 내림차순, 음수를 오름차순 정렬
+ (양수*양수) , (음수*음수) 👉🏻 이득
+ 0은 음수랑 곱해줘야 이득 
+ 1은 양수든 음수든 그냥 더해줘야 이득

# 👨🏻‍💻 CODE

```python
import sys
input = sys.stdin.readline

length = int(input())
positive = [] # 양수 배열
negative = [] # 음수 배열
one = [] # 1 배열
answer = 0 

for i in range(length):
    num = int(input())

    if num == 1:
        one.append(num)
    else:
        if num > 0:
            positive.append(num)
        
        if num <= 0:
            negative.append(num)

positive.sort(reverse=True)
negative.sort()

if len(positive) % 2 == 1:
    answer += positive.pop()

for i in range(0,len(positive),2):
    answer += positive[i]*positive[i+1]

if len(negative) % 2 == 1:
    answer += negative.pop()

for i in range(0,len(negative),2):
    answer += negative[i]*negative[i+1]

answer += len(one)

print(answer)
```



