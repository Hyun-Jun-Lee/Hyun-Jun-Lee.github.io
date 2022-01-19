---
title:  "[Greedy] 1931번 회의실 배정"
excerpt: "Baekjoon 단계별 문제"

categories:
- BAEKJOON

toc: True
toc_sticky: True

date: 2022-01-17
last_modified_at: 2022-01-17
---

# [Greedy] 1931번 회의실 배정

> 문제

한 개의 회의실이 있는데 이를 사용하고자 하는 N개의 회의에 대하여 회의실 사용표를 만들려고 한다. 각 회의 I에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 회의의 최대 개수를 찾아보자. 단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자마자 끝나는 것으로 생각하면 된다.

> 입력

첫째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 각 회의의 정보가 주어지는데 이것은 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 231-1보다 작거나 같은 자연수 또는 0이다.

> 출력

첫째 줄에 최대 사용할 수 있는 회의의 최대 개수를 출력한다.

> 예제 입력

```
11
1 4
3 5
0 6
5 7
3 8
5 9
6 10
8 11
8 12
2 13
12 14
```

> 예제 출력

4

<br>

> 풀이

```python
import sys

# sys.stdin.readline() 시간 296ms
# input() 시간 : 4392ms
n = int(sys.stdin.readline())

m_time = []

for _ in range(n):
    start, end = map(int, sys.stdin.readline().split())
    m_time.append((start, end))

# 빨리 끝나는 시간 1차 정렬 후 빨리 시작하는 시간으로 정렬
# [(1,4),(3,5),(0,6),(5,7)]
m_time = sorted(m_time, key=lambda x:(x[1], x[0]))

end_time = 0
cnt = 0
for t in m_time:
    # 시작 시간이 끝나는 시간보다 크거나 같다면
    if t[0] >= end_time:
        end_time = t[1]
        cnt += 1
print(cnt)
```

- 회의는 중간에 중단 될 수 없다.
- 회의가 끝나는 것과 동시에 다음 회의가 시작 될 수 있다
- 회의의 시작 시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자 마자 끝나는것

회의의 끝과 시작이 같을 수 있다는 것은 입력이

```
2
1 1
1 1
```

이렇게 주어져도 결과는 2가 놔와야 한다는 뜻이다.

주어진 시작시간과 끝나는 시간들을 이용해서 가장 많은 회의의 수를 알기 위해서는`빨리 끝나는 회의 순서대로 정렬`을 해야 한다. 
(빨리 끝날수록 뒤에서 고려해볼 회의가 많기 때문,빨리 시작하는 순서대로 정렬을 우선 한다면, 오히려 늦게 끝날 수 있음)

입력이 다음과 같을 때

```
4
0 10
3 4
2 3
1 2
```

시작 순서대로하면 (0 10)으로 1번의 회의가 가능하지만, 끝나는 시간으로 정렬을 한다면 (1 2) (2 3) (3 4) 총 3번의 회의가 가능해진다.

끝나는 시간이 같을 경우도 있기 때문에, 끝나는 시간이 같다면 시작 시간이 빠른 순서로 정렬 되야한다.

2
2 2
1 2

의 경우 이 상태로 한다면 (2 2)가 되고 (1 2)는 (2 2)의 끝나는 시간보다 시작시간이 일찍이기 때문에 무시되어 1번의 회의가 진행된다고 나온다. 하지만 정렬을 통해 (1 2)가 먼저 선택되면 (2 2)도 선택이 가능해지기 때문에 가능한 회의는 2번으로 결정된다.

그래서 `m_time = sorted(m_time, key=lambda x:(x[1], x[0]))` 에서 lambda를 통해 끝나는 시간(x[1])으로 정렬 후, 시작시간(x[0])으로 정렬함