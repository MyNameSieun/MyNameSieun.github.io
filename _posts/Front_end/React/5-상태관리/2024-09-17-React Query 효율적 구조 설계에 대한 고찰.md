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

---

React Query를 사용할 때 프로젝트가 커짐에 따라 코드가 복잡해지는 문제가 발생하였다.

이 문제를 해결하기 위해 고민한 결과, 코드의 복잡도를 줄이고 관리하기 쉬운 구조를 설계하는 방법에 대해 포스팅하게 되었다!

---

# 1. 쿼리 키 상수로 관리하기

> React Query에서 데이터를 패칭할 때 queryKey를 사용하여 각 데이터를 식별한다. 하지만 문자열로 직접 사용하다 보면 오타로 인한 오류가 발생할 수 있다.

따라서, 쿼리 키를 상수화하는 방법을 사용하는 것이 좋다.

```jsx
// src/components/hooks/query/keys.constant.js
export const QUERY_KEYS = {
  TODOS: "todos",
  USERS: "users",
};
```

<br><br>

# 2. 커스텀 훅으로 데이터 패칭 로직 캡슐화

> React Query를 사용하여 useQuery 등의 훅으로 데이터를 가져오는 로직을 반복하게 되면, 코드가 중복되고 유지보수가 어려워진다.

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

이제 컴포넌트에서 간단하게 커스텀 훅을 호출하여 데이터를 가져올 수 있다.

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

<br>
