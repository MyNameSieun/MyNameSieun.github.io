---
title: "[Algorithm/Java] Stack, Queue 정리"
categories: [Algorithm]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 자바 Stack vs Queue 비교 정리

| 항목        | Stack                       | Queue                                 |
| ----------- | --------------------------- | ------------------------------------- |
| 구조        | LIFO (후입선출)             | FIFO (선입선출)                       |
| 주요 클래스 | `Stack<E>`                  | `Queue<E>` (보통 `LinkedList`로 구현) |
| 대표 메서드 | `push()`, `pop()`, `peek()` | `offer()`, `poll()`, `peek()`         |
| 사용 예     | 괄호 짝 맞추기, DFS         | BFS, 작업 순서 처리                   |

<br>

## Stack 메서드

```java
Stack<Integer> stack = new Stack<>();
stack.push(1);       // 값 추가
stack.pop();         // 마지막 값 제거 및 반환
stack.peek();        // 마지막 값 확인
stack.isEmpty();     // 비었는지 확인
```

<br>

## Queue 메서드 (LinkedList 기반)

```java
Queue<Integer> q = new LinkedList<>();
q.offer(1);          // 값 추가
q.poll();            // 앞의 값 제거 및 반환
q.peek();            // 앞의 값 확인
q.isEmpty();         // 비었는지 확인
```

<br><br>

# 2. 백준 추천 문제

## Stack 관련 문제

| 문제 번호                                      | 제목      | 티어  | 설명                                                    |
| ---------------------------------------------- | --------- | ----- | ------------------------------------------------------- |
| [10828](https://www.acmicpc.net/problem/10828) | 스택      | 실버4 | 기본 스택 구현 문제                                     |
| [9012](https://www.acmicpc.net/problem/9012)   | 괄호      | 실버4 | 괄호 짝 맞추기 (`(`, `)`)                               |
| [1874](https://www.acmicpc.net/problem/1874)   | 스택 수열 | 실버2 | 스택을 통해 특정 수열을 만들 수 있는지 확인하는 문제    |
| [10773](https://www.acmicpc.net/problem/10773) | 제로      | 실버4 | 0이 나오면 직전 수를 스택에서 제거하는 방식의 구현 문제 |

<br>

## Queue 관련 문제

| 문제 번호                                      | 제목            | 티어  | 설명                                      |
| ---------------------------------------------- | --------------- | ----- | ----------------------------------------- |
| [10845](https://www.acmicpc.net/problem/10845) | 큐              | 실버4 | 큐 기본 기능 구현                         |
| [18258](https://www.acmicpc.net/problem/18258) | 큐 2            | 실버4 | 큐 대용량 입력 대응 (BufferedReader 필수) |
| [11866](https://www.acmicpc.net/problem/11866) | 요세푸스 문제 0 | 실버4 | 원형 큐처럼 동작하며 순차적으로 사람 제거 |
| [2164](https://www.acmicpc.net/problem/2164)   | 카드2           | 실버4 | 큐를 이용한 카드 제거 시뮬레이션          |

<br>
