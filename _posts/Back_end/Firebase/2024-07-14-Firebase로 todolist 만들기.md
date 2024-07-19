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

컬렉션 생성 후, 기본 item 만들어주기

![](/assets/images/2024/2024-07-15-17-35-42.png)

<br><br>

# 2. TodoList 만들기

- [[파이어스토어 문서↗️]](https://firebase.google.com/docs/firestore/quickstart?hl=ko&authuser=0&_gl=1*1yigp7a*_up*MQ..*_ga*NzA3NjY1OTMwLjE3MjEwMzA2NzU.*_ga_CW55HF8NVT*MTcyMTAzMDY3NS4xLjEuMTcyMTAzMjYzMS4yOC4wLjA.) 를 참고하여 CRUD를 구현하자
- 코드는 [[여기↗️]](https://mynamesieun.github.io/react/Todolist-%EB%8B%A4%EC%96%91%ED%95%9C-%EB%B0%A9%EB%B2%95%EC%9C%BC%EB%A1%9C-%EC%A0%84%EC%97%AD%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0/) 에 Context API를 참고하자

## 2.1 TodoContext.jsx

{% raw %}

```jsx
import { createContext, useState, useEffect } from "react";
import {
  collection,
  getDocs,
  addDoc,
  deleteDoc,
  doc,
  getDoc,
  updateDoc,
} from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
import { db } from "../firebase/firebaseConfig";

export const TodoContext = createContext();

// collection은 상수로 지정해 주는 게 좋다.
const TODOS_COLLECTION = "todos";

export const TodoContextProvider = ({ children }) => {
  const [todos, setTodos] = useState([]);

  // 데이터 읽기
  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const querySnapshot = await getDocs(collection(db, TODOS_COLLECTION));

        const nextTodos = querySnapshot.docs.map((doc) => ({
          ...doc.data(),
          id: doc.id,
        }));

        setTodos(nextTodos); // 상태 업데이트
      } catch (error) {
        console.error("데이터를 가져오는 중 에러가 발생하였습니다.", error);
      }
    };

    fetchTodos();
  }, []);

  // 데이터 추가
  const addTodo = async (nextTodo) => {
    try {
      const doc = await addDoc(collection(db, TODOS_COLLECTION), nextTodo);

      const nextTodoWithServerId = {
        ...nextTodo,
        id: doc.id,
      };

      // optimistic update(중요한 처리 할 때 사용하지 말것 ex) 결제)
      setTodos((prevTodos) => [nextTodoWithServerId, ...prevTodos]);
    } catch (e) {
      console.error("Error adding document: ", e);
    }
  };

  // 데이터 삭제
  const deleteTodo = async (id) => {
    try {
      // Firestore에서 문서 삭제
      await deleteDoc(doc(db, TODOS_COLLECTION, id));

      // 상태에서 할 일 삭제
      setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
    } catch (error) {
      console.error("할 일을 삭제하는 중 오류가 발생했습니다:", error);
    }
  };

  // 데이터 수정
  const updateTodo = async (updatedTodo) => {
    try {
      // Firestore에서 문서 참조 가져오기
      const todoRef = doc(db, TODOS_COLLECTION, updatedTodo.id);

      // Firestore에서 문서 업데이트
      await updateDoc(todoRef, {
        title: updatedTodo.title,
        content: updatedTodo.content,
        deadline: updatedTodo.deadline,
      });

      // 상태 업데이트
      setTodos((prevTodos) =>
        prevTodos.map((todo) =>
          todo.id === updatedTodo.id ? updatedTodo : todo
        )
      );
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  };

  // 할 일 완료 상태 토글
  const toggleTodo = async (id) => {
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
      value={{
        todos,
        setTodos,
        addTodo,
        deleteTodo,
        toggleTodo,
        sortTodos,
        updateTodo,
      }}
    >
      {children}
    </TodoContext.Provider>
  );
};
```

{% endraw %}

<br>

## 2.2 TodoItem.jsx

{% raw %}

```jsx
import { TodoContext } from "context/TodoContext";
import { useContext, useState } from "react";
import styled from "styled-components";

const TodoItem = ({ todo }) => {
  const { deleteTodo, toggleTodo, updateTodo } = useContext(TodoContext);

  const [edit, setEdit] = useState(null);

  // 할 일 삭제 함수
  const handleDeleteButton = async (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      try {
        await deleteTodo(id);
        alert("삭제되었습니다.");
      } catch (error) {
        console.error("할 일을 삭제하는 중 오류가 발생했습니다:", error);
      }
    }
  };

  // 할 일을 수정 모드로 변경하는 함수
  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  // 할 일 수정 완료 함수
  const handleUpdateButton = async () => {
    try {
      await updateTodo({
        id: edit.id,
        title: edit.title,
        content: edit.content,
        deadline: edit.deadline,
        isDone: edit.isDone, // optional, if you want to keep isDone
      });

      alert("수정 되셨습니다.");
      setEdit(null);
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  };

  // 할 일의 완료 상태 토글 함수
  const isDoneOnToggleButton = async (id) => {
    try {
      await toggleTodo(id);
    } catch (error) {
      console.error(
        "할 일의 완료 상태를 토글하는 중 오류가 발생했습니다:",
        error
      );
    }
  };

  // 날짜 형식을 한국어로 변환
  const formattedDate = new Date(todo.deadline).toLocaleDateString("ko-KR", {
    year: "2-digit",
    month: "long",
    day: "numeric",
    weekday: "long",
  });

  return (
    <StTodoBox>
      {edit && todo.id === edit.id ? (
        <ul>
          <input
            type="text"
            name="title"
            value={edit.title}
            onChange={(e) => setEdit({ ...edit, title: e.target.value })}
          />
          <input
            type="text"
            name="content"
            value={edit.content}
            onChange={(e) => setEdit({ ...edit, content: e.target.value })}
          />
          <input
            type="date"
            name="deadline"
            value={edit.deadline}
            onChange={(e) => setEdit({ ...edit, deadline: e.target.value })}
          />
          <button onClick={() => handleDeleteButton(todo.id)}>삭제</button>
          <button onClick={handleUpdateButton}>수정 완료</button>
        </ul>
      ) : (
        <ul>
          <li>제목: {todo.title}</li>
          <li>내용: {todo.content}</li>
          <li>마감일: {formattedDate}</li>
          <button onClick={() => handleDeleteButton(todo.id)}>삭제</button>
          <button onClick={() => handleEditButton(todo)}>수정</button>
          <button onClick={() => isDoneOnToggleButton(todo.id)}>
            {todo.isDone ? "할 일 취소" : "할 일 완료"}
          </button>
        </ul>
      )}
    </StTodoBox>
  );
};

export default TodoItem;

const StTodoBox = styled.div`
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  padding: 1rem 4rem;
  border: 1px solid black;
`;
```

{% endraw %}

<br>
