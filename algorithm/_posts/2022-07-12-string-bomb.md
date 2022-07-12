---
layout: post
title: "[백준 - 9935] 문자열 폭발"
description: >
  백준 Class4 Gold4
sitemap: false
tags: [Stack]
hide_last_modified: true
---

# 💣 문자열 폭발

solved_ac[Class4] [문자열 폭발](https://www.acmicpc.net/problem/9935)

## 문제
---
상근이는 문자열에 폭발 문자열을 심어 놓았다. 폭발 문자열이 폭발하면 그 문자는 문자열에서 사라지며, 남은 문자열은 합쳐지게 된다.

폭발은 다음과 같은 과정으로 진행된다.

- 문자열이 폭발 문자열을 포함하고 있는 경우에, 모든 폭발 문자열이 폭발하게 된다. 남은 문자열을 순서대로 이어 붙여 새로운 문자열을 만든다.
- 새로 생긴 문자열에 폭발 문자열이 포함되어 있을 수도 있다.
- 폭발은 폭발 문자열이 문자열에 없을 때까지 계속된다.

상근이는 모든 폭발이 끝난 후에 어떤 문자열이 남는지 구해보려고 한다. 남아있는 문자가 없는 경우가 있다. 이때는 "FRULA"를 출력한다.

폭발 문자열은 같은 문자를 두 개 이상 포함하지 않는다.

## 입력
---
첫째 줄에 문자열이 주어진다. 문자열의 길이는 1보다 크거나 같고, 1,000,000보다 작거나 같다.

둘째 줄에 폭발 문자열이 주어진다. 길이는 1보다 크거나 같고, 36보다 작거나 같다.

두 문자열은 모두 알파벳 소문자와 대문자, 숫자 0, 1, ..., 9로만 이루어져 있다.

## 출력
---
첫째 줄에 모든 폭발이 끝난 후 남은 문자열을 출력한다.

## 예제 입력 1 

```
mirkovC4nizCC44
C4
```

## 예제 출력 1 

```
mirkovniz
```

## 예제 입력 2

```
12ab112ab2ab
12ab
```

## 예제 출력 2

```
FRULA
```

# 📖 자료구조

**[문자열에서 특정 문자열을 연속적으로 제거해나갈때 스택을 사용할 수 있다.]**

스택을 사용한다는 사실을 알아차리지 못 하면 풀기가 상당히 까다롭다.

무려 3시간 동안 삽질을 했다 ... 😭 

## TRY 1

![연습장-23](https://user-images.githubusercontent.com/88064555/178479822-229927c2-ef9c-4670-993f-269c885f45fe.jpg)

👉🏻 TLE

join 함수를 이용하여 string 형태를 유지하며 문자열을 제거해나가는 방식이다.

join 함수는 높은 시간복잡도를 갖기 때문에 시간 초과에 걸리게 된다.

## TRY 2

![연습장-24](https://user-images.githubusercontent.com/88064555/178480409-b8a2b274-0416-42ee-b3e0-d74aec3d9e0d.jpg)

👉🏻 구현 못 하였음.

예제를 살펴보자.

원본 문자열: 12ab112ab2ab , 폭탄 문자열: 12ab

+ 문자열을 리스트 형태로 바꾼다.
+ 원본 리스트를 폭탄 리스트와 비교해보면서 겹치는 부분을 공백으로 치환
+ 이를 반복
+ 최종 리스트를 문자열로 변환 (join 함수를 한 번만 사용하기 때문에 시간복잡도를 줄일 수 있을 것이라 판단)

2번 단계에서 3번 단계로 넘어가는 것을 구현하기가 어려워 포기함.

## TRY 3

![연습장-25](https://user-images.githubusercontent.com/88064555/178481006-024c1128-103c-4de0-813e-06c6e9bf439e.jpg)

👉🏻 정답

원본 문자열에서 문자를 하나씩 꺼내어 스택에 담아가면서 폭탄 문자열이 완성됐을 때 POP을 시킨다.

그렇게 하여 최종 스택을 문자열 형태로 출력하면 정답!

# 👨🏻‍💻 CODE

## TRY 1

```python
### 문제 조건 ###
string = input()
bomb = input()
######


def bomb_check(i,string,bomb):
    for c in bomb:
        if string[i] != c:
            return False
        i += 1
    return True

while True:
    save_string = string
    # 문자열 폭발부
    for i in range(len(string)):
        if string[i] == bomb[0]:
            if bomb_check(i,string,bomb):
                temp = list(string)
                for j in range(len(bomb)):
                    temp[i+j] = " "
                string = "".join(temp)
    string = string.replace(" ","")

    if save_string == string:
        break

if string == "":
    print("FRULA")
else:
    print(string)
```

## TRY 3

```python
### 문제 조건 ###
string = []
temp = input()
for i in range(len(temp)):
    string.append(temp[i])
bomb = []
temp = input()
for i in range(len(temp)):
    bomb.append(temp[i])
######

stack = []
count = 0

for i in range(len(string)):
    stack.append(string[i])
    count += 1

    if count >= len(bomb):
        for j in range(count-len(bomb),count):
            if stack[j] != bomb[j-count]:
                break
        else:
            for k in range(len(bomb)):
                stack.pop()
                count -= 1

if not stack:
    print("FRULA")
else:
    print("".join(stack))
```



