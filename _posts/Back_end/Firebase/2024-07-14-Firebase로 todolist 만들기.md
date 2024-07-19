---
title: "[Firebase/React] Firebase로 todolist 만들기"
categories: [Firebase]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 개요

## 1.1 Firebase 세팅하기

> Firebase 세팅

[[여기↗️]](https://mynamesieun.github.io/firebase/Firebase-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/) 링크를 참고하여 파이어베이스를 세팅하자!

> Firebase 설치

```
yarn add firebase
```

<br>

## 1.2 FireStore 시작하기

> FireStore 시작
> [[여기↗️]](https://mynamesieun.github.io/firebase/Firestore-DB/#-%ED%8C%8C%EC%9D%B4%EC%96%B4%EC%8A%A4%ED%86%A0%EC%96%B4-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0) 링크를 참고하여 파이어스토어를 시작하자

> 컬렉션 생성 후, 기본 item 만들어주기

![](/assets/images/2024/2024-07-15-17-35-42.png)

<br><br>

# 2. TodoList 만들기

- [[파이어스토어 문서↗️]](https://firebase.google.com/docs/firestore/quickstart?hl=ko&authuser=0&_gl=1*1yigp7a*_up*MQ..*_ga*NzA3NjY1OTMwLjE3MjEwMzA2NzU.*_ga_CW55HF8NVT*MTcyMTAzMDY3NS4xLjEuMTcyMTAzMjYzMS4yOC4wLjA.) 를 참고하여 CRUD를 구현하자
- 코드는 [[여기↗️]](https://mynamesieun.github.io/react/Todolist-%EB%8B%A4%EC%96%91%ED%95%9C-%EB%B0%A9%EB%B2%95%EC%9C%BC%EB%A1%9C-%EC%A0%84%EC%97%AD%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0/) 에 Context API를 참고하자

# 1. 데이터 읽기

```jsx
import { createContext, useState, useEffect } from "react";
import { v4 as uuid } from "uuid";

import { collection } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
import { getDocs } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
import { db } from "../firebase/firebaseConfig";

export const TodoContext = createContext();

// collection은 상수로 지정해 주는 게 좋다.
const TODOS_COLLECTION = "todos";

export const TodoContextProvider = ({ children }) => {
  const [todos, setTodos] = useState([]);

  // 데이터 읽기
  useEffect(() => {
    const fetchTodos = async () => {
      const querySnapshot = await getDocs(collection(db, TODOS_COLLECTION));
      const nextTodos = querySnapshot.docs.map((doc) => ({
        id: doc.id,
        ...doc.data(),
      }));
      setTodos(nextTodos);
    };

    fetchTodos();
  }, []);

  // 할 일 추가
  const addTodo = (newTodo) => {
    setTodos((prevTodos) => [
      ...prevTodos,
      { ...newTodo, id: uuid(), isDone: false },
    ]);
  };

  // 할 일 삭제
  const deleteTodo = (id) => {
    setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
  };

  // 할 일 완료 상태 토글
  const toggleTodo = (id) => {
    setTodos((prevTodos) =>
      prevTodos.map((todo) =>
        todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
      )
    );
  };

  // 할 일 정렬(마감일 기준)
  const sortTodos = (order) => {
    if (order === "asc") {
      setTodos((prevTodos) =>
        [...prevTodos].sort(
          (a, b) => new Date(a.deadline) - new Date(b.deadline)
        )
      );
    } else if (order === "desc") {
      setTodos((prevTodos) =>
        [...prevTodos].sort(
          (a, b) => new Date(b.deadline) - new Date(a.deadline)
        )
      );
    }
  };

  return (
    <TodoContext.Provider
      value={{ todos, setTodos, addTodo, deleteTodo, toggleTodo, sortTodos }}
    >
      {children}
    </TodoContext.Provider>
  );
};
```

<br>

# 2. 데이터 추가

```jsx
const onSubmitTodo = async (nextTodo) => {
  const doc = await addDoc(collection(db, TODOS_COLLECTION), nextTodo);

  const nextTodoWithServerId = {
    ...nextTodo,
    id: doc.id,
  };

  // optimistic update
  setTodos((prevTodos) => [nextTodoWithServerId, ...prevTodos]);
};
```

<br>

# 3. 데이터 삭제

```jsx
const onDeleteTodoItem = async (id) => {
  await deleteDoc(doc(db, TODOS_COLLECTION, id));

  setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
};
```

<br>

# 4. 데이터 수정

```jsx
const onToggleTodoItem = async (id) => {
  // 서버의 상태를 가져와서 업데이트
  const todoRef = doc(db, TODOS_COLLECTION, id);
  const docSnap = await getDoc(todoRef);

  await updateDoc(todoRef, {
    isDone: !docSnap.data().isDone,
  });

  // 또는 클라이언트의 데이터를 가져와서 업데이트
  // const todo = todos.find((todoItem) => todoItem.id === id);
  // await updateDoc(doc(db, TODOS_COLLECTION, id), {
  //   isDone: !todo.isDone,
  // });

  setTodos((prevTodos) =>
    prevTodos.map((todoItem) => {
      if (todoItem.id === id) {
        return {
          ...todoItem,
          isDone: !todoItem.isDone,
        };
      }

      return todoItem;
    })
  );
};
```

<br>
