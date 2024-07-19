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

<br><br>

# 3. TodoList CRUD 정리

## 3.1 CREATE

> Optimistic Update 사용

- Optimistic Update는 서버의 응답을 기다리지 않고 UI를 미리 업데이트하는 방법이다.
- 더 빠른 반응을 제공하여 사용자 경험(UX)을 향상시키는 효과가 있다.

{% raw %}

```jsx
// 새로운 Todo를 추가하는 비동기 함수
const onSubmitTodo = async (nextTodo) => {
  // Firestore의 'todos' 컬렉션에 새로운 문서를 추가
  // `nextTodo`는 추가할 할 일 객체
  const ref = await addDoc(collection(db, TODOS_COLLECTION), nextTodo);

  // Firestore 서버에서 생성된 문서의 ID를 가져와서 원래 객체에 추가
  // 이렇게 하면 로컬 상태에서도 Firestore 서버의 ID를 사용할 수 있게 됨-> 데이터 일관성
  const nextTodoWithServerId = {
    ...nextTodo, // 기존의 할 일 객체를 spread operator로 복사해 넣어주기
    id: ref.id, // Firestore에서 생성된 문서 ID를 추가
  };

  // Optimistic Update를 수행
  setTodos((prevTodos) => [nextTodoWithServerId, ...prevTodos]);
};
```

{% endraw %}

<br>

> Optimistic Update 사용x

- Optimistic Update를 사용하지 않는 코드는 서버로부터 응답을 받은 후에만 UI를 업데이트하므로, 서버와의 데이터 일관성은 확실히 유지
- 오류가 발생할 경우를 대비해 try-catch 블록을 추가하여 오류를 처리

{% raw %}

```jsx
const onSubmitTodo = async (nextTodo) => {
  try {
    const ref = await addDoc(collection(db, TODOS_COLLECTION), nextTodo);

    const nextTodoWithServerId = {
      ...nextTodo,
      id: ref.id,
    };

    // Firestore 서버에서 문서 추가가 완료된 후 상태를 업데이트
    setTodos((prevTodos) => [nextTodoWithServerId, ...prevTodos]);
  } catch (error) {
    // 오류가 발생하면 오류를 처리
    console.error("Error adding document: ", error);
  }
};
```

{% endraw %}

<br>

## 3.2 READ

{% raw %}

```jsx
// useEffect 훅을 사용하여 컴포넌트가 마운트될 때 한 번만 실행되도록 합니다.
useEffect(() => {
  // Firestore에서 투두 리스트를 비동기적으로 가져오는 함수
  const fetchTodos = async () => {
    // 'todos' 컬렉션에서 문서들을 가져옵니다.
    const querySnapshot = await getDocs(collection(db, TODOS_COLLECTION));

    // 가져온 문서들을 순회하면서, 각 문서의 데이터와 문서 ID를 객체로 변환하여 배열에 담습니다.
    const nextTodos = querySnapshot.docs.map((doc) => ({
      ...doc.data(), // 문서의 데이터를 객체로 가져옵니다.
      id: doc.id, // 문서의 ID를 추가합니다. 데이터를 식별하는 데 사용됩니다.
    }));

    // 변환된 객체 배열을 사용하여 로컬 상태를 업데이트합니다.
    setTodos(nextTodos);

    // forEach를 사용한 방법
    // 빈 배열을 생성합니다. 이 배열에는 Firestore에서 가져온 각 문서의 데이터가 저장됩니다.
    // const nextTodos = []

    // querySnapshot의 각 문서에 대해 반복합니다.
    // querySnapshot.forEach((doc) => {
    //   console.log(`${doc.id} => ${doc.data()}`); // 콘솔에 문서 ID와 데이터를 출력합니다.
    //   const data = doc.data(); // 문서의 데이터를 가져옵니다.

    //   // 가져온 데이터와 문서 ID를 객체로 만들어 nextTodos 배열에 추가합니다.
    //   nextTodos.push({
    //     ...data, // 문서의 데이터
    //     id: doc.id, // 문서의 ID
    //   });
    // });
  };

  // 함수를 호출합니다.
  fetchTodos();
}, []); // 빈 의존성 배열을 전달하여, 이 useEffect 훅이 컴포넌트가 마운트될 때 단 한 번만 실행되도록 합니다.
```

{% endraw %}

<br>

## 3.3 UPDATE

{% raw %}

```jsx
// 투두 아이템의 완료 상태를 토글하는 비동기 함수
const onToggleTodoItem = async (id) => {
  // Firestore에서 업데이트할 데이터를 가져옵니다.
  const todoRef = doc(db, TODOS_COLLECTION, id);

  // 해당 문서의 현재 상태를 가져옵니다.
  const docSnap = await getDoc(todoRef);

  // 또는 로컬 데이터를 가져와서 업데이트합니다.
  // const todo = todos.find((todoItem) => todoItem.id === id);
  // await updateDoc(doc(db, TODOS_COLLECTION, id), {
  //   isDone: !todo.isDone,
  // });

  // 문서의 `isDone` 필드를 현재 값의 반대로 설정하여 업데이트합니다.
  await updateDoc(todoRef, {
    isDone: !docSnap.data().isDone,
  });

  // 로컬 상태도 업데이트합니다.
  // `prevTodos`는 업데이트 이전의 최신 상태입니다.
  setTodos((prevTodos) =>
    prevTodos.map((todoItem) => {
      // 변경하려는 투두 아이템의 ID가 일치하는 경우
      if (todoItem.id === id) {
        // 해당 항목의 `isDone` 상태를 토글합니다.
        return {
          ...todoItem,
          isDone: !todoItem.isDone,
        };
      }

      // ID가 일치하지 않는 항목은 변경 없이 그대로 반환합니다.
      return todoItem;
    })
  );
};
```

{% endraw %}

<br>

## 3.4 DELETE

{% raw %}

```jsx
// 투두 아이템을 삭제하는 비동기 함수입니다.
const onDeleteTodoItem = async (id) => {
  // Firestore에서 해당 ID를 가진 할 일 문서를 삭제합니다.
  await deleteDoc(doc(db, TODOS_COLLECTION, id));

  // 삭제 후, 로컬 상태를 업데이트합니다.
  setTodos((prevTodos) =>
    // `prevTodos`는 삭제하기 전의 최신 상태입니다.
    // `.filter()` 메소드를 사용하여, 삭제된 항목을 제외한 나머지 항목만을 새로운 배열로 반환합니다.
    // `todo.id !== id` 조건은 현재 처리 중인 항목의 ID가 삭제하려는 항목의 ID와 다른 경우에만 true를 반환합니다.
    // 즉, 삭제하려는 항목을 제외하고 나머지 항목들로만 구성된 새로운 배열이 생성됩니다.
    prevTodos.filter((todo) => todo.id !== id)
  );
};
```

{% endraw %}

<br>
