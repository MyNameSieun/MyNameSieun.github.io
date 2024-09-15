---
title: "[React] React Query로 TodoList 리팩토링하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. React Query로 todolist 만들기

## 1.1 설치

```shell
yarn add @tanstack/react-query
```

<br>

## 1.2 설정

> App root에 `QueryClient`와 `QueryClientProvider`를 설정하여 앱 전체에서 eact Query를 사용할 수 있도록 해주자.

```jsx
// index.jsx or App.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import GlobalStyle from "styles/GlobalStyle";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const root = ReactDOM.createRoot(document.getElementById("root"));

const queryClient = new QueryClient();

root.render(
  <React.StrictMode>
    <GlobalStyle />
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

<br>

# 2. API 요청

> 서버와의 통신을 위한 API 요청 로직을 별도로 관리하는 것이 좋다. 따라서 src/api/todos.js 파일에서 Axios 인스턴스를 생성하고 API 요청 함수를 정의하였다.

```jsx
// src/api/todos.js
import axios from "axios";

const todosAxios = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL,
});

// todos 조회
export const fetchTodos = async () => {
  const response = await todosAxios.get("/todos");
  return response.data;
};

// todos 작성
export const addTodos = async (data) => {
  const response = await todosAxios.post("/todos", data);
  return response.data;
};

// todos 삭제
export const deleteTodos = async (id) => {
  const response = await todosAxios.delete(`/todos/${id}`);
  return response.data;
};

// todos 수정
export const editTodos = async (id, data) => {
  const response = await todosAxios.patch(`/todos/${id}`, data);
  return response.data;
};

// todos toggle
export const toggleTodos = async (id, isDone) => {
  const response = await todosAxios.patch(`/todos/${id}`, { isDone: !isDone });
  return response.data;
};
```

<br><br>

# 3. Todos.jsx

## 3.1 기존 코드

> 기존의 Todos 컴포넌트는 `useEffect`와 `useState`를 사용하여 데이터를 로드하고 업데이트했다.

```jsx
import TodoForm from "./TodoForm";
import TodoList from "./TodoList";

import { fetchTodos } from "api/todos";
import { useEffect, useState } from "react";

const Todos = () => {
  const [todos, setTodos] = useState([]);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    const loadTodos = async () => {
      try {
        const response = await fetchTodos();
        setTodos(response.data);
      } catch (error) {
        console.error(error);
      } finally {
        setIsLoading(false);
      }
    };
    loadTodos();
  }, []);

  if (isLoading) {
    return <p>로딩중...</p>;
  }

  return (
    <>
      <TodoForm setTodos={setTodos} />
      <h1>할 일 남음ㅜㅜ</h1>
      <TodoList todos={todos} setTodos={setTodos} isDone={false} />
      <h1>할 일 완료!</h1>
      <TodoList todos={todos} setTodos={setTodos} isDone={true} />
    </>
  );
};

export default Todos;
```

## 3.2 React Query 사용

> React Query의 `useQuery` 훅을 사용하여 데이터를 가져오고, `useMutation` 훅을 사용하여 데이터를 업데이트한다.

GET 에는 useQuery를, PUT, UPDATE, DELETE에는 useMutation을 사용한다.

```jsx
// src/components/Todos/Todos.jsx
// src/components/Todos/Todos.jsx
import { useQuery } from "@tanstack/react-query";
import TodoForm from "./TodoForm";
import TodoList from "./TodoList";
import { fetchTodos } from "api/todos";

const Todos = () => {
  // todos 데이터 fetch
  const {
    data: todos = [],
    isLoading,
    error,
  } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  // 로딩 중 처리
  if (isLoading) {
    return <p>로딩중...</p>;
  }

  // 에러 중 처리
  if (error) {
    return <p>에러 발생: {error.message}</p>;
  }

  // 완료되지 않은 항목과 완료된 항목을 필터링
  const todosNotDone = todos.filter((todo) => !todo.isDone); // 할 일 남음
  const todosDone = todos.filter((todo) => todo.isDone); // 할 일 완료

  return (
    <>
      <TodoForm />
      <h1>할 일 남음</h1>
      <TodoList todos={todosNotDone} isDone={false} />
      <h1>할 일 완료!</h1>
      <TodoList todos={todosDone} isDone={true} />
    </>
  );
};

export default Todos;
```

<br>

# 4. TodoForm.jsx

## 4.1 기존 코드

> 기존의 TodoForm 컴포넌트는 폼 제출 시 데이터를 API에 전송하고 setTodos를 사용하여 상태를 업데이트했다.

```jsx
// src/components/Todos/TodoForm.jsx
import { addTodos, fetchTodos } from "api/todos";
import { useState } from "react";

const TodoForm = ({ setTodos }) => {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      const newTodo = { title, content, isDone: false };
      await addTodos(newTodo);
      alert("추가 완료!");
      setTitle("");
      setContent("");
      const response = await fetchTodos();
      setTodos(response.data);
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="title">제목: </label>
        <input
          id="title"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="content">내용: </label>
        <input
          id="content"
          value={content}
          onChange={(e) => setContent(e.target.value)}
        />
      </div>
      <button type="submit">제출</button>
    </form>
  );
};

export default TodoForm;
```

## 4.2 React Query 사용

> `useMutation`을 사용하여 Todo 항목을 추가하고 쿼리 무효화를 통해 데이터를 새로고침한다.

```jsx
// src/components/Todos/TodoForm.jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { addTodos } from "api/todos";
import { useState } from "react";

const TodoForm = () => {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");

  const queryClient = useQueryClient(); // queryClient를 사용하여 쿼리 무효화

  const addTodoMutation = useMutation({
    mutationFn: addTodos,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ["todos"] }); // 'todos' 쿼리를 무효화하여 목록을 새로고침
      setTitle(""); // 입력 필드 초기화
      setContent(""); // 입력 필드 초기화
    },
  });

  const handleSubmit = async (e) => {
    e.preventDefault();
    // 할 일 추가
    addTodoMutation.mutate({ title, content, isDone: false }); // addTodos 호출
    alert("작성 완료!");
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="title">제목: </label>
        <input
          id="title"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          required
        />
      </div>
      <div>
        <label htmlFor="content">내용: </label>
        <input
          id="content"
          value={content}
          onChange={(e) => setContent(e.target.value)}
          required
        />
      </div>
      <button type="submit">제출</button>
    </form>
  );
};

export default TodoForm;
```

<br>

# 5. TodoList.jsx

## 5.1 기존 코드

> 기존의 TodoList 컴포넌트는 각 Todo 항목을 렌더링하고 상태를 업데이트했다.

```jsx
// src/components/Todos/TodosList.jsx
import { deleteTodos, editTodos, fetchTodos, toggleTodos } from "api/todos";
import { useState } from "react";

const TodoList = ({ todos, setTodos, isDone }) => {
  const [editTodo, setEditTodo] = useState(null);

  const filteredTodos = todos.filter((todo) => todo.isDone === isDone); // Filter todos based on isDone status

  // 삭제
  const handleDeleteButton = async (id) => {
    try {
      await deleteTodos(id);
      alert("삭제 완료!");
      const response = await fetchTodos();
      setTodos(response.data);
    } catch (error) {
      console.error(error);
    }
  };

  // 수정
  const handleEditTodos = async (id) => {
    try {
      await editTodos(id, { title: editTodo.title, content: editTodo.content });
      alert("수정 완료!");
      setEditTodo(null);
      const response = await fetchTodos();
      setTodos(response.data);
    } catch (error) {
      console.error(error);
    }
  };

  const handleToggleTodos = async (id, isDone) => {
    try {
      toggleTodos(id, isDone);
      const response = await fetchTodos();
      setTodos(response.data);
    } catch (error) {
      console.error(error);
    }
  };

  return (
    <ul>
      {filteredTodos.map((todo) => (
        <li key={todo.id}>
          {editTodo && todo.id === editTodo.id ? (
            <>
              <input
                value={editTodo.title}
                onChange={(e) =>
                  setEditTodo((prev) => ({ ...prev, title: e.target.value }))
                }
                placeholder="수정할 제목을 입력하세요"
              />
              <input
                value={editTodo.content}
                onChange={(e) =>
                  setEditTodo((prev) => ({ ...prev, content: e.target.value }))
                }
                placeholder="수정할 내용을 입력하세요"
              />
              <button onClick={() => setEditTodo(null)}>수정 취소</button>
              <button onClick={() => handleEditTodos(todo.id)}>
                수정 완료
              </button>
            </>
          ) : (
            <>
              <p>제목: {todo.title}</p>
              <p>내용: {todo.content}</p>
              <button onClick={() => handleDeleteButton(todo.id)}>삭제</button>
              <button onClick={() => setEditTodo(todo)}>수정</button>
              <button onClick={() => handleToggleTodos(todo.id, todo.isDone)}>
                {isDone ? "할 일 취소" : "할 일 완료"}
              </button>
            </>
          )}
        </li>
      ))}
    </ul>
  );
};

export default TodoList;
```

<br>

## 5.2 React Query 사용

> `useMutation`을 사용하여 Todo 항목을 삭제, 수정 및 토글한다.

```jsx
// src/components/Todos/TodosList.jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { deleteTodos, editTodos, toggleTodos } from "api/todos";
import { useState } from "react";

const TodoList = ({ todos, isDone }) => {
  const [editTodo, setEditTodo] = useState(null);
  const queryClient = useQueryClient(); // 캐시 업데이트를 위한 queryClient 사용

  // 삭제 확인 처리
  const handleDelete = (id) => {
    const isConfirmed = window.confirm("정말로 삭제하시겠습니까?");
    if (isConfirmed) {
      deleteTodoMutation.mutate(id);
    }
  };
  // 삭제
  const deleteTodoMutation = useMutation({
    mutationFn: deleteTodos,
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]); // 'todos' 쿼리를 무효화하여 목록을 새로고침
      alert("삭제 완료!");
    },
  });

  // 수정
  const editTodoMutation = useMutation({
    mutationFn: ({ id, updatedTodo }) => editTodos(id, updatedTodo),
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]); // 'todos' 쿼리 무효화
      setEditTodo(null); // 수정 완료 후 수정 상태 초기화
      alert("수정 완료!");
    },
  });

  // 토글 기능 (완료/미완료 상태 변경)
  const toggleTodoMutation = useMutation({
    mutationFn: ({ id, isDone }) => toggleTodos(id, isDone),
    onSuccess: () => {
      queryClient.invalidateQueries(["todos"]); // 'todos' 쿼리를 무효화하여 데이터를 새로고침
    },
  });

  // isDone 상태에 따라 필터링
  const filteredTodos = todos.filter((todo) => todo.isDone === isDone);

  return (
    <ul>
      {filteredTodos.map((todo) => (
        <li key={todo.id}>
          {editTodo && todo.id === editTodo.id ? (
            <>
              <input
                value={editTodo.title}
                onChange={(e) =>
                  setEditTodo((prev) => ({ ...prev, title: e.target.value }))
                }
                placeholder="수정할 제목을 입력하세요"
              />
              <input
                value={editTodo.content}
                onChange={(e) =>
                  setEditTodo((prev) => ({ ...prev, content: e.target.value }))
                }
                placeholder="수정할 내용을 입력하세요"
              />
              <button onClick={() => setEditTodo(null)}>수정 취소</button>
              <button
                onClick={() =>
                  editTodoMutation.mutate({
                    id: todo.id,
                    updatedTodo: editTodo,
                  })
                }
              >
                수정 완료
              </button>
            </>
          ) : (
            <>
              <p>제목: {todo.title}</p>
              <p>내용: {todo.content}</p>
              <button onClick={() => handleDelete(todo.id)}>삭제</button>
              <button onClick={() => setEditTodo(todo)}>수정</button>
              <button
                onClick={() =>
                  toggleTodoMutation.mutate({
                    id: todo.id,
                    isDone: todo.isDone, // 현재 isDone 상태를 전달
                  })
                }
              >
                {isDone ? "할 일 취소" : "할 일 완료"}
              </button>
            </>
          )}
        </li>
      ))}
    </ul>
  );
};

export default TodoList;
```

<br>
