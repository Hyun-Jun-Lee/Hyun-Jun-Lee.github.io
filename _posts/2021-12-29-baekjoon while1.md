---
title:  "[While] A+B-5"
excerpt: "Baekjoon 단계별 문제"

categories:
- BAEKJOON

toc: True
toc_sticky: True

date: 2021-12-29
last_modified_at: 2021-12-29
---

# [While] A+B-5

> 문제

두 정수 A와 B를 입력받은 다음, A+B를 출력하는 프로그램을 작성

> 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다.

각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

입력의 마지막에는 0 두 개가 들어온다.

> 출력

각 테스트 케이스마다 A+B 출력

<br>

> 풀이

```python
while True:
    a,b = map(int,input().split())
    if a==0 and b==0:
        break
    print(a+b)
```

- `While True`로 while문을 무한루프로
- a,b를 int 형으로 입력 받고 a,b가 둘다 0인 경우 while문 탈출
- a+b값 출력
