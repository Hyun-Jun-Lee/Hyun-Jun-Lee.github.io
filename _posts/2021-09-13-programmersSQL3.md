---
title:  "[SELECT] 아픈 동물 찾기 "
excerpt: "Programmers SQL "

categories:
- Programmers SQL

toc: false
toc_sticky: false

date: 2021-09-13
last_modified_at: 2021-09-13
---

> 문제

![image](https://user-images.githubusercontent.com/76996686/133087653-5f37500b-8edb-4e36-b13f-bad73c24977e.png)


<br>

> 답

```sql

SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
ORDER BY ANIMAL_ID ASC

```