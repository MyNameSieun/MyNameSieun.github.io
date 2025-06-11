---
title: "[Algorithm/Java] Stack, Queue, Deque 메서드 정리"
categories: [Algorithm]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Stack vs Queue vs Deque 메서드 정리

## Stack vs Queue vs Deque

- 웬만한 코딩테스트에서는 Deque 하나만으로 Queue 역할을 완벽하게 대체할 수 있다.
- 덱은 큐와 달리,
  - 1. 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료 구조
  - 2. 두 개의 포인터를 사용하여, 앞(front)과 뒤(back) 양쪽 모두에서 원소를 추가하거나 제거 가능
  - 3. 큐(FIFO)와 스택(LIFO)의 특징을 모두 가지고 있는 하이브리드 형태

| 항목        | Stack                       | Queue                                 | Deque                                                                               |
| ----------- | --------------------------- | ------------------------------------- | ----------------------------------------------------------------------------------- |
| 구조        | LIFO (후입선출)             | FIFO (선입선출)                       | 양방향 큐 (Double Ended Queue)                                                      |
| 주요 클래스 | `Stack<E>`                  | `Queue<E>` (보통 `LinkedList`로 구현) | `Deque<E>` (보통 `ArrayDeque`로 구현)                                               |
| 대표 메서드 | `push()`, `pop()`, `peek()` | `offer()`, `poll()`, `peek()`         | `addFirst()`, `addLast()`, `pollFirst()`, `pollLast()`, `peekFirst()`, `peekLast()` |
| 사용 예     | 괄호 짝 맞추기, DFS         | BFS, 작업 순서 처리                   | 양방향 탐색, 회문 검사, 슬라이딩 윈도우, 투 포인터 등                               |

<br>

## Queue vs Deque 메서드 비교

| 기능        | 일반 큐 (`Queue`) | 덱 (`Deque`)   |
| ----------- | ----------------- | -------------- |
| 앞에서 삽입 | 불가능            | `offerFirst()` |
| 앞에서 삭제 | `poll()`          | `pollFirst()`  |
| 뒤에서 삽입 | `offer()`         | `offerLast()`  |
| 뒤에서 삭제 | 불가능            | `pollLast()`   |
| 앞 조회     | `peek()`          | `peekFirst()`  |
| 뒤 조회     | 불가능            | `peekLast()`   |

- `Deque`는 `Queue` 인터페이스를 **상속**받고 있으므로, `Deque`로 선언해도 `Queue` 메서드들 (`offer`, `poll`, `peek`)을 그대로 쓸 수 있음
- 사실 `offer() == offerLast()` 라고 봐도 됨
- 메서드 명이 다른이유는 `Deque`에서는 **명시적으로 방향을 드러낸 이름**으로 제공되기 때문

![](/assets/images/2025/2025-06-11-16-20-52.png)

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

<br>

## Deque 메서드

```java
Deque<Integer> dq = new ArrayDeque<>();

dq.addFirst(1);     // 앞에 추가
dq.addLast(2);      // 뒤에 추가
dq.pollFirst();     // 앞에서 제거 및 반환
dq.pollLast();      // 뒤에서 제거 및 반환
dq.peekFirst();     // 앞 값 확인
dq.peekLast();      // 뒤 값 확인
dq.isEmpty();       // 비었는지 확인
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

## Deque 관련 문제

| 문제 번호                                      | 제목        | 티어  | 설명                                                |
| ---------------------------------------------- | ----------- | ----- | --------------------------------------------------- |
| [10866](https://www.acmicpc.net/problem/10866) | 덱          | 실버4 | 덱 기본 기능 구현 문제                              |
| [1021](https://www.acmicpc.net/problem/1021)   | 회전하는 큐 | 실버3 | 덱을 이용한 왼쪽/오른쪽 회전 시뮬레이션             |
| [5430](https://www.acmicpc.net/problem/5430)   | AC          | 골드5 | 덱을 이용한 문자열 명령어 처리 (R: 뒤집기, D: 제거) |
| [28279](https://www.acmicpc.net/problem/28279) | 덱 2        | 실버4 | 큐 2처럼 대용량 입력에 대응하는 덱 구현 문제        |

<br>
