---
title:  "코딩 테스트 스터디 1일차 - 공통문제"
excerpt: "2020 카카오 인턴십 코딩 테스트 문제 1. 키패드 누르기"

categories:
  - CodingTestStudy
tags:
  - [Python, CodingTest]

toc: true
toc_sticky: true
 
date: 2021-07-18
last_modified_at: 2021-07-21
---

# 2020 카카오 인턴십 코딩 테스트 문제 1. 키패드 누르기
## 문제 출처
https://programmers.co.kr/learn/courses/30/lessons/67256

<br/>

## 문제 해설 
<br/>

``` python
key_loc = {1: (1, 1), 2: (1, 2), 3: (1, 3), 4: (2, 1),
           5: (2, 2), 6: (2, 3), 7: (3, 1), 8: (3, 2),
           9: (3, 3), 0: (4, 2), '*': (4, 1), '#': (4, 3)}
```

왼쪽 엄지 손가락으로는 1, 4, 7의 키패드를, 오른쪽 엄지 손가락으로는 3, 6, 9 키패드를 누르면 되는데, 2, 5, 8, 0은 두 엄지 손가락의 위치에서 가장 가까운 엄지손가락을 찾아 누르도록 해야하기 때문에 거리를 계산하기 위해 각 키패드의 위치를 나타내는 dictionary로 key: 각 키패드 번호, value: 배열 형태의 위치로 나타냈다. 

<br/>

``` python
def distance(key1, key2):
    return abs(key_loc[key1][0] - key_loc[key2][0]) + abs(key_loc[key1][1] - key_loc[key2][1])
```

또한 2, 5, 8, 0을 입력할 때 두 엄지 손가락의 현재 위치에서 다음 입력할 숫자와의 거리를 비교해야 하기 때문에 distance 함수를 만들어 주었다.

<br/>

``` python
def solution(numbers, hand):
    answer = ''
    left_loc = '*'
    right_loc = '#'
```
answer는 입력의 결과를 반환할 변수이고, 초기 두 엄지 손가락의 위치는 * 와 # 키패드에 위치하기 때문에 두 엄지 손가락의 위치 변수로 *, #으로 초기화 한 후 입력할 숫자들을 순차적으로 진행하였다.

<br/>

``` python
for i in numbers:
        if i in [1, 4, 7]:
            answer += 'L'
            left_loc = i
        elif i in [3, 6, 9]:
            answer += 'R'
            right_loc = i
```
다음 입력할 숫자가 1, 4, 7이면 결과에 L을 추가하고 현재 왼쪽 손가락의 위치를 해당 숫자로 바꾸어주고, 3, 6, 9이면 결과에 R을 추가하고 현재 오른쪽 손가락의 위치를 해당 숫자로 바꾸어준다.

<br/>

```python
        else:
            if distance(left_loc, i) < distance(right_loc, i):
                answer += 'L'
                left_loc = i
            elif distance(left_loc, i) > distance(right_loc, i):
                answer += 'R'
                right_loc = i
```
다음 입력할 숫자가 2, 5, 8, 0이면, <br/>
1. 현재 왼쪽 엄지 손가락의 위치와 다음 입력할 키패드 사이의 거리
2. 현재 오른쪽 엄지 손가락의 위치와 다음 입력할 키패드 사이의 거리<br/>

1, 2를 비교하고 가까운 손가락으로 결과값에 L, R을 추가해주고 손가락의 위치를 해당 숫자로 바꾸어준다.

<br/>

```python
            else:
                if hand == 'right':
                    answer += 'R'
                    right_loc = i
                else:
                    answer += 'L'
                    left_loc = i
    return answer
```

1, 2번의 값이 서로 같을 수가 있기 때문에 같을 경우에 자기의 손잡이로 입력 받은 hand 변수에 따라 오른손잡이면 R, 왼손잡이면 L을 결과값에 추가해주고 손가락의 위치를 해당 숫자로 바꾸어준다.<br/>
입력할 숫자가 더이상 없게 되면 결과값을 반환한다.

<br/>

제출 결과는 성공이었고 전체 코드는 다음과 같다.

```python
key_loc = {1: (1, 1), 2: (1, 2), 3: (1, 3), 4: (2, 1),
           5: (2, 2), 6: (2, 3), 7: (3, 1), 8: (3, 2),
           9: (3, 3), 0: (4, 2), '*': (4, 1), '#': (4, 3)}


def solution(numbers, hand):
    answer = ''
    left_loc = '*'
    right_loc = '#'

    for i in numbers:
        if i in [1, 4, 7]:
            answer += 'L'
            left_loc = i
        elif i in [3, 6, 9]:
            answer += 'R'
            right_loc = i
        else:
            if distance(left_loc, i) < distance(right_loc, i):
                answer += 'L'
                left_loc = i
            elif distance(left_loc, i) > distance(right_loc, i):
                answer += 'R'
                right_loc = i
            else:
                if hand == 'right':
                    answer += 'R'
                    right_loc = i
                else:
                    answer += 'L'
                    left_loc = i
    return answer


def distance(key1, key2):
    return abs(key_loc[key1][0] - key_loc[key2][0]) + abs(key_loc[key1][1] - key_loc[key2][1])
```