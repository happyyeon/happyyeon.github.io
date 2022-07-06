---
layout: post
title:  "[백준 9375] - 패션왕 신해빈"
tags: [Dictionary,조합]
hide_last_modified: true
---

# 패션왕 신해빈

solved_ac[Class3] [패션왕 신해빈](https://www.acmicpc.net/problem/9375)

## 문제

해빈이는 패션에 매우 민감해서 한번 입었던 옷들의 조합을 절대 다시 입지 않는다. 예를 들어 오늘 해빈이가 안경, 코트, 상의, 신발을 입었다면, 다음날은 바지를 추가로 입거나 안경대신 렌즈를 착용하거나 해야한다. 해빈이가 가진 의상들이 주어졌을때 과연 해빈이는 알몸이 아닌 상태로 며칠동안 밖에 돌아다닐 수 있을까?

## 입력

첫째 줄에 테스트 케이스가 주어진다. 테스트 케이스는 최대 100이다.

각 테스트 케이스의 첫째 줄에는 해빈이가 가진 의상의 수 n(0 ≤ n ≤ 30)이 주어진다.
다음 n개에는 해빈이가 가진 의상의 이름과 의상의 종류가 공백으로 구분되어 주어진다. 같은 종류의 의상은 하나만 입을 수 있다.
모든 문자열은 1이상 20이하의 알파벳 소문자로 이루어져있으며 같은 이름을 가진 의상은 존재하지 않는다.

## 출력

각 테스트 케이스에 대해 해빈이가 알몸이 아닌 상태로 의상을 입을 수 있는 경우를 출력하시오.

+ 예제 입력 1 

```
2
3
hat headgear
sunglasses eyewear
turban headgear
3
mask face
sunglasses face
makeup face
```

+ 예제 출력 1 

```
5
3
```

## 힌트

첫 번째 테스트 케이스는 headgear에 해당하는 의상이 hat, turban이며 eyewear에 해당하는 의상이 sunglasses이므로   (hat), (turban), (sunglasses), (hat,sunglasses), (turban,sunglasses)로 총 5가지 이다.

# Dictionary

<img width="808" alt="스크린샷 2022-05-22 오전 6 33 24" src="https://user-images.githubusercontent.com/88064555/169669692-a741a75a-62b3-4d76-9598-4883b4cf50b1.png">

딕셔너리를 사용하는 이유는 Input을 받기 위해서이다.
정수 형태의 Input이라면 리스트를 사용해서 Index를 이용해서 풀어도 되었겠지만 문자열 입력이기에 딕셔너리를 사용한다.

# 조합

각 카테고리에서 1개의 의상을 선택하는 것이기 때문에 aC1 * bC1 * cC1 ... = a * b * c ... 가 전체 경우의 수이고
옷을 안 입은 경우 (x,x,...)는 제외 시켜야하기 때문에 1을 빼준다.

# CODE

```python
import sys
input = sys.stdin.readline

# 옷 정보 Dictionary
for _ in range(int(input())):
    clothes_dict = dict()
    for _ in range(int(input())):
        clothes, category = input().split()
        
        if category in clothes_dict.keys():
            clothes_dict[category].append(clothes)
        else:
            clothes_dict[category] = [clothes]

    answer = 1

    for category in clothes_dict.keys():
        answer *= len(clothes_dict[category])+1

    print(answer-1)
```

