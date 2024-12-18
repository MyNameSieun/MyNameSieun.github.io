---
title: "[Python] 파이썬 타입 어노테이션"
categories: [Python]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 타입 어노테이션

## 1.1 타입 어노테이션이란?

> 타입 어노테이션(type annotation)이란, 타입에 대한 힌트를 알려주는 기능이다.

어노테이션을 사용해 변수의 예상 타입을 지정하여 프로그래머가 개발 과정에서 의도한 타입에 대한 정보를 명확하게 전달할 수 있게 도와준다.

<br>

## 1.2 함수 어노테이션션

> 기본 문법

```python
def 함수이름(매개변수: 타입) -> 반환타입:
    ...
```

<br>

> 예시

```python
def add(a: int, b: int) -> int:
    return a + b
```

- `a: int`와 `b: int`는 매개변수의 타입을 나타낸다.
- `-> int`는 반환값의 타입을 나타냅니다.

```python
num: int = 1
def add(a: int, b:int) -> int:
    return a+b

res = add(3, 8)
```

<br>

## 1.3 변수 어노테이션

> 기본 문법

```python
변수이름: 타입 = 값
```

<br>

> 예시

```python
age: int = 25
name: str = "Alice"
```

<br>
