---
layout: post
title:  "[백준 6064] - 카잉 달력"
tags: [최소공배수]
hide_last_modified: true
---

# 카잉 달력

solved_ac[Class3] [카잉 달력](https://www.acmicpc.net/problem/6064)

## 문제

최근에 ICPC 탐사대는 남아메리카의 잉카 제국이 놀라운 문명을 지닌 카잉 제국을 토대로 하여 세워졌다는 사실을 발견했다. 카잉 제국의 백성들은 특이한 달력을 사용한 것으로 알려져 있다. 그들은 M과 N보다 작거나 같은 두 개의 자연수 x, y를 가지고 각 년도를 <x:y>와 같은 형식으로 표현하였다. 그들은 이 세상의 시초에 해당하는 첫 번째 해를 <1:1>로 표현하고, 두 번째 해를 <2:2>로 표현하였다. <x:y>의 다음 해를 표현한 것을 <x':y'>이라고 하자. 만일 x < M 이면 x' = x + 1이고, 그렇지 않으면 x' = 1이다. 같은 방식으로 만일 y < N이면 y' = y + 1이고, 그렇지 않으면 y' = 1이다. <M:N>은 그들 달력의 마지막 해로서, 이 해에 세상의 종말이 도래한다는 예언이 전해 온다. 

예를 들어, M = 10 이고 N = 12라고 하자. 첫 번째 해는 <1:1>로 표현되고, 11번째 해는 <1:11>로 표현된다. <3:1>은 13번째 해를 나타내고, <10:12>는 마지막인 60번째 해를 나타낸다. 

네 개의 정수 M, N, x와 y가 주어질 때, <M:N>이 카잉 달력의 마지막 해라고 하면 <x:y>는 몇 번째 해를 나타내는지 구하는 프로그램을 작성하라. 

## 입력

입력 데이터는 표준 입력을 사용한다. 입력은 T개의 테스트 데이터로 구성된다. 입력의 첫 번째 줄에는 입력 데이터의 수를 나타내는 정수 T가 주어진다. 각 테스트 데이터는 한 줄로 구성된다. 각 줄에는 네 개의 정수 M, N, x와 y가 주어진다. (1 ≤ M, N ≤ 40,000, 1 ≤ x ≤ M, 1 ≤ y ≤ N) 여기서 <M:N>은 카잉 달력의 마지막 해를 나타낸다.

## 출력

출력은 표준 출력을 사용한다. 각 테스트 데이터에 대해, 정수 k를 한 줄에 출력한다. 여기서 k는 <x:y>가 k번째 해를 나타내는 것을 의미한다. 만일 <x:y>에 의해 표현되는 해가 없다면, 즉, <x:y>가 유효하지 않은 표현이면, -1을 출력한다.

## 예제 입력 1 

```
3
10 12 3 9
10 12 7 2
13 11 5 6
```

## 예제 출력 1 

```
33
-1
83
```
---
---

# 가져다 쓰기

해당 문제에서는 특별히 가져다 쓴 알고리즘이 없다. 문제 조건에 맞는 규칙을 찾는 것이 포인트였다.

## 최소공배수

메인은 아니지만 문제를 푸는 과정에서 최소공배수를 사용해야할 일이 있었는데 

공식을 사용하여 다음과 같이 구현하였다.

```python
# 최소공배수
def lcm(x,y):
    return x*y // gcd(x,y)
```
---
---

# 과정 설계

## 1트
<img width="963" alt="스크린샷 2022-06-03 오후 11 47 26" src="https://user-images.githubusercontent.com/88064555/171877849-9cfe581d-c9c0-40d3-a701-7eea5a531e31.png">

처음에는 배열을 사용하여 접근하였다. (x:y)를 구하기 위하여 x가 나올 수 있는 모든 연수 배열을 구하고 

마찬가지로 y가 나올 수 있는 모든 연수 배열을 구하여 두 배열의 공통 집합을 찾았다.

### Pseudo CODE
<img width="731" alt="스크린샷 2022-06-03 오후 11 51 41" src="https://user-images.githubusercontent.com/88064555/171878595-df742e56-381a-42c7-8703-a0ca90376aa1.png">

### CODE

```python
import sys
from math import gcd
input = sys.stdin.readline

# 최소공배수
def lcm(x,y):
    return x*y // gcd(x,y)

for _ in range(int(input())): # Test case
    m,n,x,y = map(int,input().split()) # M,N,X,Y 입력
    last_num = lcm(m,n)
    
    # [x ~ lcm(x,y)] 배열 생성
    arr1 = [x]
    i = m
    while x+i <= last_num:
        arr1.append(x+i)
        i += m
    
    # [y ~ lcm(x,y)] 배열 생성
    arr2 = [y]
    j = n
    while y+j <= last_num:
        arr2.append(y+j)
        j += n
    
    # print(arr1)
    # print(arr2)

    # 2개의 배열 겹치는 원소 탐색
    for num in arr1:
        if num in arr2: # 있으면 출력
            print(num)
            break
    else:# 없으면 -1 출력
        print(-1)
```
결과는 "시간 초과" 배열 전체를 탐색하는 O(n)의 시간복잡도 때문이라고 예상해 본다.

## 2트

<img width="1120" alt="스크린샷 2022-06-03 오후 11 55 04" src="https://user-images.githubusercontent.com/88064555/171879124-4c7decc9-ef25-41b9-9587-e83b69d8b33c.png">

시간적으로 좀 더 효율적으로 풀기 위하여 x와 y가 어떤 연관 관계가 있는지 파악하였다.

그림을 참고해보면 x를 고정시켜놓고 y가 어떻게 변화하는지를 알 수 있다.

[M=10, N=12, x=3, y=9]일 때, x=3으로 고정시켜놓고 나올 수 있는 모든 y를 순차적으로 계산한다.

그러다가 우리가 찾고자 하는 y=9가 나오면 멈추고 해당 연수를 출력해주면 답이다.

### CODE

```python
import sys
from math import gcd
input = sys.stdin.readline

# 최소공배수
def lcm(x,y):
    return x*y // gcd(x,y)

for _ in range(int(input())): # Test case
    m,n,x,y = map(int,input().split()) # M,N,X,Y 입력
    last_num = lcm(m,n)
    
    temp_y = x
    year = x

    while temp_y != y:
        temp_y = (temp_y+m)%n
        year += m

        if year > last_num:
            break
            
    if year > last_num:
        print(-1)
    else:
        print(year)
```

## 3트

반례를 못 찾아서 한참 헤맸다. 

[M=13, N=11, x=1,y=1]일 때 output 66이 출력되어야 하는데 -1이 출력되는 반례를 찾았다. 

그 이유인 즉슨 아래 그림과 같다.

![caing_calendar-7](https://user-images.githubusercontent.com/88064555/171881675-b2d44d92-4b00-4c1e-9ad6-028081579ed9.jpg)

```python
    while temp_y != y:
        temp_y = (temp_y+m)%n
        year += m
```

while문 조건을 걸 때 temp_y가 y랑 같아질 때까지로 설정하였다. 이렇게 되면 문제가 

나머지가 0이 될 때 0 != 11 이므로 정답을 무시하고 계속 탐색하게 된다.

이를 해결하기 위해서 위의 코드를 아래와 같이 변경하였다.

```python
    while temp_y%n != y%n:
        temp_y = (temp_y+m)%n
        year += m
```

나머지 연산으로 비교해주면 이러한 반례를 해결할 수 있다.

---
---
# 정답 CODE

```python
import sys
from math import gcd
input = sys.stdin.readline

# 최소공배수
def lcm(x,y):
    return x*y // gcd(x,y)

for _ in range(int(input())): # Test case
    m,n,x,y = map(int,input().split()) # M,N,X,Y 입력
    last_num = lcm(m,n)
    
    temp_y = x
    year = x

    while temp_y%n != y%n:
        temp_y = (temp_y+m)%n
        year += m

        if year > last_num:
            break
            
    if year > last_num:
        print(-1)
    else:
        print(year)
```


