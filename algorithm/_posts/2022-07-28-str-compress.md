---
layout: post
title: "[프로그래머스] 문자열 압축"
description: >
  프로그래머스 Level2
sitemap: false
tags: [Brute Force]
hide_last_modified: true
---

# 💬 문자열 압축

# 📖 가져다 쓰기

처음에는 스택을 떠올렸다. 왜냐하면 문자열에서 특정 문자열을 검색하는 문제이기 때문이다. 

그러나 시간복잡도를 구해보고 브루트 포스로 풀어야겠다고 생각했다. 

+ 입력 데이터 개수: 1000 👉🏻 O(N^2*logN)까지 가능
+ 브루트 포스로 예상되는 시간 복잡도 👉🏻 O(N^2)

# 📐 과정 설계/관리

![연습장-63](https://user-images.githubusercontent.com/88064555/181485252-1849708b-fcbe-4cd6-86f2-df371ecad727.jpg)


입력: aabbaccc가 들어왔을 때를 예시로 진행해보겠다.

먼저 1부터 절반까지 문자열을 나누는 모든 경우를 구한다. 이 때, 절반까지 탐색하는 이유는 어차피 절반을 넘어가는 순간 문자열 압축이 되지 않기 때문이다.

(그런데 최종적인 코드는 불필요하더라도 1부터 전체를 나누어 풀었다. 그 이유는 문자열 길이가 1일때는 절반으로 쪼개지지 않기 때문에 에러가 발생한다.)

그렇게 하여 각각의 압축된 문자열 길이를 비교하여 최소값을 반환해주면 정답이다.


![연습장-64](https://user-images.githubusercontent.com/88064555/181485322-650d8dfd-34c9-4417-a68c-71ee0562023a.jpg)

![연습장-65](https://user-images.githubusercontent.com/88064555/181485372-19362a11-703d-498d-940d-781786b7da2a.jpg)


이제부터는 나누어진 각 문자열에 대하여 어떻게 압축이 진행되는지를 설명해보도록 하겠다.

가장 첫 번째 인덱스부터 시작하여 다음 인덱스를 비교해보아 같은 값이 나오면 count를 증가시키며 계속 탐색한다.

만약 count가 1이라면 그대로 압축 문자열에 붙여주고 2 이상이라면 count와 함께 같이 붙여준다.

그렇게 문자열 끝까지 탐색을 마치면 최종적인 압축 문자열이 생성되고 이를 정답 후보군 리스트에 넣어준다.

마지막에 정답 후보군 리스트에서 최소값을 선택해주면 그것이 곧 정답 !!!

# 👨🏻‍💻 CODE

```python
def solution(s):
    candidates = []

    # 문자열 분리
    for a in range(1,len(s)+1):
        i=0
        arr = []
        while i+a<len(s):
            arr.append(s[i:i+a])
            i += a
        arr.append(s[i:])

        answer = ""
        i=0
        while 0<=i<len(arr):
            count = 1
            if i != len(arr)-1:
                while arr[i] == arr[i+1]:
                    count += 1
                    i += 1

                    if i == len(arr)-1:
                        break
            
            if count == 1:
                answer += arr[i]
            else:
                answer += str(count) + arr[i]
            
            i += 1

        candidates.append(len(answer))

    return min(candidates)
```
