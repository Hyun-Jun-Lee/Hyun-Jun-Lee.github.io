---
title:  "CPU & 메모리"
excerpt: "CPU 작동원리, Memory"

categories:
- CS

toc: True
toc_sticky: True

date: 2022-01-14
last_modified_at: 2022-04-07
---

# CPU 작동원리 & Cache Memory

## CPU

### CPU의 구성 요소

CPU는 컴퓨터에서 가장 핵심적인 역활을 수행하는 부분으로 연산장치, 제어장치, 레지스터로 구성된다.

- 연산 장치
  - 산술, 논리 연산을 수행
  - 연산에 필요한 데이터를 레지스터에서 가져오고, 연산 결과를 다시 레지스터로 보냄

- 제어 장치
  - 명령어를 순서대로 실행할 수 있도록 제어하는 장치
  - 주기억장치에서 프로그램 명령어를 꺼낸 후 해독하고, 그 결과에 따라 명령어 실행에 필요한 제어신로를 기억장치, 연산장치, 입출력장치로 보냄
  - 그리고 이들 장치가 보낸 신호를 받아 다음 수행할 동작을 결정

- 레지스터
  - 고속 기억장치
  - 명령어 주소, 코드, 연산에 필요한 데이터, 연산결과 등을 임시로 저장
  - CPU 종류에 따라 사용할 수 있는 레지스터 개수와 크기가 다름
    - 범용 레지스터 : 연산에 필요한 데이터나 연산 결과를 임시로 저장
    - 특수목적 레지스터 : 특별한 용도로 사용하는 레지스터
      - MAR(메모리 주소 레지스터) : 읽기와 쓰기 연산을 수행할 주기억장치 주소 저장
      - PC(프로그램 카운터) : 다음에 수행할 명령어 주소 저장
      - IR(명령어 레지스터) : 현재 실행 중인 명령어 저장
      - MBR(메모리 버퍼 레지스터) : 주기억장치에서 읽어온 데이터 or 저장할 데이터 임시 저장
      - AC(누산기) : 연산 결과 임시 저장

### CPU 동작 과정

1. 주기억장치는 입력 장치에서 입력받은 데이터 또는 보조기억장치에서 저장된 프로그램을 읽어온다.

2. CPU는 프로그램을 실행하기 위해 주기억장치에 저장된 프로그램 명령어와 데이터를 읽어와 처리하고 결과를 다시 주기억장치에 저장

3. 주기억장치는 처리 결과를 보조기억장치에 저장하거나 출력장치로 보냄

4. 제어장치는 1~3 과정에서 명령어가 순서대로 실행되도록 각 장치를 제어한다.


CPU는 프로그램을 실행하기 위해 주기억장치에서 명령어를 순차적으로 인출하여 해독하고 실행하는 과정을 반복하며, CPU가 주기억장치에서 한 번에 하나의 명령어를 인출하여 실행하는데 필요한 일련의 활동을 '명령어 사이클' 이라고한다.

<br>

## 메모리

### 보조 기억장치 (HDD,SSD)

전원이 꺼져도 데이터가 지워지지않는 `비휘발성 메모리`지만 데이터 `처리 속도가 느리다`. 과거에는 CD, DVD가 있었지만 지금은 하드디스크가 담당한다.

### 주기억장치(RAM, 메인 메모리)

현재 `실행중`인 프로그램의 데이터나 처리중인 결과를 저장한다. 

`하드 디스크로부터 일정량의 데이터를 복사해서 저장하고 필요할 때 마다 CPU에 전달`하는 역활

전원이 꺼지면 데이터가 지워지는 `휘발성 메모리`이지만, 데이터 `처리속도가 빠르다.`

> 프로그램 실행이 늦는 이유?

프로그램이 실행 되려면 `하드디스크에서 데이터를 읽어 RAM으로 전송(로딩)`해야 한다. 그래서 RAM의 용량이 적거나 느리면 프로그램의 로딩이 길어진다.

### Cache Memory

속도가 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위한 메모리

ex) 웹 브라우저 캐시 파일은, 하드디스크와 웹페이지 사이의 병목 현상을 완화

> 병목 현상 : 어떤 시스템 내 데이터의 집중적인 사용으로 인해, 전체 시스템에 절대적 영향을 미치는 부분의 사용 빈도가 늘어나 그 부분의 성능이 저하되어 전체 시스템이 마비되는 현상

CPU가 주기억장치에서 저장된 데이터를 읽어올 때, `자주 사용하는 데이터를 캐시 메모리에 저장`한 뒤 다음에 `필요할 때 주기억장치가 아닌 캐시 메모리에서 먼저 가져오는 방식`으로 속도를 향상 시킨다. 

but 용량이 적고 비용이 비쌈(SRAM가격이 매우 비싸기 때문)

CPU에는 이와 같읔 캐시 메모리가 2~3개 정도 사용되고, 속도와 크기에 따라 L1,L2,L3로 분류된다.

- L1 : CPU 내부에 존재
- L2 : CPU와 RAM 사이에 존재
- L3 : 보통 메인보드에 존재

#### Cache Memory 작동 원리

- 시간 지역성 : for, while 같은 반복문에 사용하는 조건 변수처럼 한 번 참조된 데이터는 잠시 후 또 참조될 가능성이 높다.
- 공간 지역성 : a[0],a[1]과 같은 연속 접근 시, 참조된 데이터 근처에 있는 데이터가 잠시 후 또 사용될 가능성이 높다.

`Chche Hit` : CPU가 요청한 데이터가 캐시에 있을 경우
`Cache Miss` : 캐시 메모리에 없어서 DRAM에서 가져온 경우

#### Cache Miss

- Cold miss : 해당 메모리 주소를 처음 불러서 나는 miss
- Conflict miss : 캐시 메모리에 A와 B 데이터를 저장해야 하는데, A와B가 같은 캐시 메모리 주소에 할당되어 있어서 나는 miss
- Capacity miss : 캐시 메모리 공간 부족으로 나는 miss

> Conflict는 주소 할당 문제, Capacity는 공간 문제

## 프로그램 실행 과정 간단하게 요약

1. 프로그램을 실행하면, 운영체제가 디스크에 있는 해당 프로그램을  찾아서 실행하여 프로그램 코드들을 메모리에 올린 `프로세스` 를 만들고 `사용자 메모리 공간`에 배치한다.


2. 해당 프로세스를 제어할 때 필요한 모든 정보를 모아서 `PCB`를 만든 다음 `커널 메모리 공간`에 배치한다.


3. 최근에 실행한 프로세스일 수록 해당 프로세스의 PCB에는 높은 우선순위가 부여된다. 운영체제의 `프로세스 스케줄러`는 PCB에 적힌 우선순위에 따라 `ready queue`에 배치한다.


4. 기존에 실행중이던 프로세스의 "시간 할당량"이 끝나면 바로 "프로세서(CPU)"를 다음 순위의 프로세스에게 줘 실행한다.


5. 프로그램 실행 완료






