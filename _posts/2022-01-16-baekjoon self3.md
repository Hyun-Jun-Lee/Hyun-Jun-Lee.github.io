---
title:  "[재귀] 2447번 별 찍기 10"
excerpt: "Baekjoon 단계별 문제"

categories:
- BAEKJOON

toc: True
toc_sticky: True

date: 2022-01-16
last_modified_at: 2022-01-16
---

# [재귀] 2447번 별 찍기 10

> 문제

재귀적인 패턴으로 별을 찍어 보자. N이 3의 거듭제곱(3, 9, 27, ...)이라고 할 때, 크기 N의 패턴은 N×N 정사각형 모양이다.

크기 3의 패턴은 가운데에 공백이 있고, 가운데를 제외한 모든 칸에 별이 하나씩 있는 패턴이다.

```
***
* *
***
```

N이 3보다 클 경우, 크기 N의 패턴은 공백으로 채워진 가운데의 (N/3)×(N/3) 정사각형을 크기 N/3의 패턴으로 둘러싼 형태이다. 예를 들어 크기 27의 패턴은 예제 출력 1과 같다.

> 입력

첫째 줄에 n이 주어진다. n은 20보다 작거나 같은 자연수 또는 0이다.

> 출력

첫째 줄에 n번째 피보나치 수를 출력한다.

> 예제 입력

10

> 예제 출력

55

<br>

> 풀이

```python
n = int(input())

def fibo(n):
    if n ==0:
        return 0
    elif n==1:
        return 1
    return fibo(n-1)+fibo(n-2)

print(fibo(n))
```
