---
title: "[TS] extends와 as를 활용하여 Todo App 개선하기"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

Todo App에서 `extends`와 `as` 키워드를 사용하여 코드의 구조를 개선해보자.

<br>

# 1. extends를 통한 인터페이스 확장

> TypeScript의 `extends` 키워드는 인터페이스를 상속하는 데 사용된다.

기본 인터페이스의 속성을 재사용하고, 특정 속성을 추가하거나 수정할 수 있다.

<br>

> Todo 인터페이스를 기반으로 두 가지 하위 인터페이스인 `DoneTodo`와 `InProgressTodo`를 만들어 보자.

각각 isDone 속성이 true 또는 false인 경우에만 사용될 수 있다.

```tsx
// src/types/todo.type.ts

export interface Todo {
  id: string;
  title: string;
  content: string;
  isDone: boolean;
  deadline: string;
}

export interface DoneTodo extends Todo {
  isDone: true; // isDone이 true일 때만 허용
}

export interface InProgressTodo extends Todo {
  isDone: false; // isDone이 false일 때만 허용
}
```

<br><br>

# 2. as를 통한 타입 단언

## 2.1 as 활용 전

> `as`를 활용하기 전 코드 예시

```tsx
const inProgressTodos = todos.filter((todo) => !todo.isDone);
const doneTodos = todos.filter((todo) => todo.isDone);
```

<br>

## 2.1 as 활용 후

> as 키워드는 타입 단언(type assertion)에 사용되며, TypeScript에게 특정 값이 특정 타입이라고 알려준다.

예를 들어, `todos` 배열을 필터링하여 진행 중인 할 일(inProgressTodos)과 완료된 할 일(doneTodos)을 구분할 때 사용한다.

```tsx
const inProgressTodos = todos.filter(
  (todo) => !todo.isDone
) as InProgressTodo[];

const doneTodos = todos.filter((todo) => todo.isDone) as DoneTodo[];
```

`as InProgressTodo[]`는 `inProgressTodos`가 `InProgressTodo` 배열임을 TypeScript에게 명시하여, 배열에 대한 타입 검사를 더욱 정확하게 한다.

<br>
