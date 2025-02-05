---
title: "[React] React Query 효율적 구조 설계에 대한 고찰"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. React Query 효율적 구조 설계 개요

> React Query를 사용할 때 프로젝트가 커짐에 따라 코드가 복잡해지는 문제가 발생하였다.

이 문제를 해결하기 위해 고민한 결과, 코드의 복잡도를 줄이고 관리하기 쉬운 구조를 설계하는 방법에 대해 포스팅하게 되었다!

<br>

> 파일 구조는 다음과 같다.

- React Query와 관련된 기능을 모듈화하여 관리하기 쉽게 나누었다.
- TypeScript를 사용하는 경우, `.js` 확장자를 `.ts`로 변경하면 된다.

```
📂 components
└───📂 hooks
    └───📂 query
        ├───📄 keys.constant.js      # 쿼리 키 상수를 정의한 파일
        ├───📄 usePostsQuery.js      # 포스트 목록을 가져오는 쿼리 훅
        └───📄 usePostsMutation.js   # 포스트 추가 및 삭제를 처리하는 뮤테이션 훅
📂 types
└───📂 post-types.js               # 포스트 관련 타입 정의
📂 api
└───📂 posts.js                   # 포스트 관련 API 호출 함수
```

<br><br>

# 2. 쿼리 키 상수로 관리하기

> React Query에서 데이터를 패칭할 때 `queryKey`를 사용하여 각 데이터를 식별한다. 하지만 문자열로 직접 사용하다 보면 오타로 인한 오류가 발생할 수 있다.

따라서, 쿼리 키를 상수화하여 관리하는 것이 좋다.

```jsx
// src/components/hooks/query/keys.constant.js
export const QUERY_KEYS = {
  TODOS: "todos",
  USERS: "users",
};
```

<br><br>

# 3. useQuery 커스텀 훅

## 3.1 useQuery 커스텀 훅 생성

> React Query를 사용하여 `useQuery`로 데이터를 가져오는 로직을 반복하게 되면, 코드가 중복되고 유지보수가 어려워진다.

아래처럼 커스텀 훅을 활용하여 데이터 패칭 로직을 컴포넌트에서 분리하면, 컴포넌트는 데이터 로딩 상태와 에러만 관리할 수 있다.

```jsx
// src/components/hooks/query/useTodosQuery.js
import { useQuery } from "@tanstack/react-query";
import { QUERY_KEYS } from "./keys.constant";
import { fetchTodos } from "api/todos";

export const useTodosQuery = () => {
  return useQuery({
    queryKey: [QUERY_KEYS.TODOS],
    queryFn: fetchTodos,
  });
};
```

<br>

## 3.2 useQuery 커스텀 훅 사용

> 이제 컴포넌트에서 간단하게 useQuery 커스텀 훅을 호출하여 데이터를 가져올 수 있다.

```jsx
// src/components/Todos/Todos.jsx
import { useTodosQuery } from "components/hooks/query/useTodosQuery";

const Todos = () => {
  const { data: todos = [], isLoading, error } = useTodosQuery(); // 커스텀 훅 호출

  if (isLoading) return <p>로딩 중...</p>;
  if (error) return <p>에러 발생: {error.message}</p>;

  return (
    <div>
      {todos.map((todo) => (
        <p key={todo.id}>{todo.title}</p>
      ))}
    </div>
  );
};

export default Todos;
```

<br><br>

# 4. useMutation 커스텀 훅

## 4.1 useMutation 커스텀 훅 생성

> `useQuery`와 마찬가지로, 커스텀 훅을 통해 `useMutation`의 로직을 캡슐화하여 컴포넌트에서 쉽게 사용할 수 있도록

```jsx
// app/hooks/query/useTodosMutation.js
import { addTodo, deleteTodo } from "@/app/services/todos";
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { QUERY_KEYS } from "./keys.constant";

export const useAddTodoMutation = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: addTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.TODOS] });
      alert("추가 완료!");
    },
    onError: (error) => {
      console.error("추가 실패:", error);
      alert("추가 실패. 다시 시도해 주세요.");
    },
  });
};

export const useDeleteTodoMutation = () => {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: deleteTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.TODOS] });
      alert("삭제 완료!");
    },
    onError: (error) => {
      console.error("삭제 실패:", error);
      alert("삭제 실패. 다시 시도해 주세요.");
    },
  });
};
```

<br>

## 4.2 useMutation 커스텀 훅 사용

> 이제 컴포넌트에서 간단하게 `useMutation` 커스텀 훅을 호출하여 데이터를 변경할 수 있다.

```jsx
"use client";

import { useDeleteTodoMutation } from "@/app/hooks/query/useTodosMutation";
import { useTodosQuery } from "@/app/hooks/query/useTodosQuery";
import { Todo } from "@/app/types/todo-types";

const HomePage = () => {
  // Todo 목록 패칭
  const { data: todos, isLoading, error } = useTodosQuery();

  // 삭제를 위한 뮤테이션 훅
  const deleteTodoMutation = useDeleteTodoMutation();

  const handleDeleteTodo = (id: string) => {
    deleteTodoMutation.mutate(id); // Todo 삭제
  };

  // 로딩 및 에러 처리
  if (isLoading) return <p>로딩중...</p>;
  if (error) return <p>에러 발생: {error.message}</p>;

  return (
    <section>
      <h1>샘플 Todo App</h1>

      {todos.map((todo: Todo) => (
        <ul key={todo.id}>
          <li>제목: {todo.title}</li>
          <li>내용: {todo.content}</li>
          <button onClick={() => handleDeleteTodo(todo.id)}>삭제하기</button>
        </ul>
      ))}
    </section>
  );
};

export default HomePage;
```

<br>

## 4.3 하나의 훅으로 여러 Mutation 관리하기

> useAddTodoMutation 훅에서는 삭제(delete), 수정(edit) Mutation을 모두 다룰 수 있도록 리팩터링 하였다.

이를 통해 여러 `Mutation` 로직을 한 번에 관리할 수 있다.

```tsx
export const useAddTodoMutation = () => {
  const queryClient = useQueryClient();

  // Delete Todo Mutation
  const { mutate: deleteTodoMutate } = useMutation({
    mutationFn: deleteTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.TODOS] });
      alert("삭제 완료!");
    },
    onError: (error) => {
      console.error("삭제 실패:", error);
      alert("삭제 실패. 다시 시도해 주세요.");
    },
  });

  // Edit Todo Mutation
  const { mutate: editTodoMutate } = useMutation({
    mutationFn: ({ id, title, content }: EditTodo & { id: string }) =>
      editTodo(id, { title, content }),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.TODOS] });
      alert("수정 완료!");
    },
    onError: (error) => {
      console.error("수정 실패:", error);
      alert("수정 실패. 다시 시도해 주세요.");
    },
  });

  return { deleteTodoMutate, editTodoMutate };
};
```

<br>

> useAddTodoMutation 훅을 사용한 예시이다! (로직을 보여주기 위해 많이 생략된 코드이다.)

```tsx
// components/todo/TodoItem.tsx
"use cilent";

const { deleteTodoMutate, editTodoMutate, toggleTodoMutate } =
  useAddTodoMutation();

// 삭제
const handleDeleteTodo = () => {
  const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
  if (deleteConfirm) {
    deleteTodoMutate(id);
  }
};

// 수정
const handleEditTodo = () => {
  if (editMode?.title && editMode?.content) {
    editTodoMutate({ id, title: editMode.title, content: editMode.content });
    setEditMode(null);
  } else {
    alert("수정할 제목과 내용을 입력하세요.");
  }
};

return (
  <>
    <button onClick={handleDeleteTodo}>삭제하기</button>
    <button onClick={() => setEditMode({ title, content })}>수정</button>
  </>
);
```

<br>
