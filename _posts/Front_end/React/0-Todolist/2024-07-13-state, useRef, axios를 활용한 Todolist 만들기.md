---
title: "[React] state, useRef, axios를 활용한 Todolist 만들기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> ⚠️ setTodos를 호출할 때 이전 상태를 기반으로 새 상태를 생성하여 불변성을 유지해야 함에 주의하자!!

<br>

# 1. state 사용하기

## 1.1 Home.jsx

```jsx
import TodoForm from "components/TodoForm";
import TodoList from "components/TodoList";
import TodoSort from "components/TodoSort";
import { useState } from "react";
import styled from "styled-components";

const Home = () => {
  const [todos, setTodos] = useState([]);

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>
      <TodoForm setTodos={setTodos} />
      <TodoSort setTodos={setTodos} />

      <StH2>미완료</StH2>
      <TodoList todos={workingTodos} setTodos={setTodos} />

      <StH2>할 일 완료</StH2>
      <TodoList todos={doneTodos} setTodos={setTodos} />
    </StHomeLayout>
  );
};

export default Home;

const StHomeLayout = styled.main`
  display: flex;
  flex-direction: column;
  align-items: center;

  height: 100vh;
`;
const StH1 = styled.h1`
  font-size: 3rem;
  font-weight: bold;

  padding: 3rem;
`;

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

## 1.2 TodoForm.jsx

```jsx
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = ({ setTodos }) => {
  const onSubmit = (e) => {
    e.preventDefault();

    const title = e.target.title.value;
    const content = e.target.content.value;
    const deadline = e.target.deadline.value;

    if (!title || !content || !deadline) {
      alert("값을 입력하세요");
      return;
    }

    const addTodos = {
      id: uuid(),
      title,
      content,
      isDone: false,
      deadline,
    };

    setTodos((prev) => [...prev, addTodos]);

    // 입력폼 초기화
    e.target.title.value = "";
    e.target.content.value = "";
    e.target.deadline.value = "";
  };

  return (
    <StTodoForm onSubmit={onSubmit}>
      <input type="text" name="title" placeholder="제목" />
      <input type="text" name="content" placeholder="내용" />
      <input type="date" name="deadline" />
      <button>추가</button>
    </StTodoForm>
  );
};

export default TodoForm;

const StTodoForm = styled.form`
  padding: 3rem;
`;
```

<br>

## 1.3 TodoList.jsx

```jsx
import TodoItem from "./TodoItem";

const TodoList = ({ todos, setTodos }) => {
  return (
    <>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} setTodos={setTodos} />
      ))}
    </>
  );
};

export default TodoList;
```

<br>

## 1.4 TodoItem.jsx

```jsx
import { useState } from "react";
import styled from "styled-components";

const TodoItem = ({ todo, setTodos }) => {
  const [edit, setEdit] = useState(null);

  const handleDeleteButton = (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      // ⚠️  setTodos를 호출할 때 이전 상태를 기반으로 새 상태를 생성하여 불변성을 유지해야 함
      setTodos((prev) => prev.filter((todo) => todo.id !== id));
      alert("삭제 되셨습니다.");
    }
  };

  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  const handleUpdateButton = () => {
    setTodos((prev) =>
      prev.map((todo) =>
        todo.id === edit.id
          ? {
              ...todo,
              title: edit.title,
              content: edit.content,
              deadline: edit.deadline,
            }
          : todo
      )
    );
    alert("수정 되셨습니다.");
    setEdit(null);
  };

  const isDoneOnToggleButton = (id) => {
    setTodos((prev) =>
      prev.map((todo) =>
        todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
      )
    );
  };

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

<br>

## 1.5 TodoSort.jsx

```jsx
import { useState } from "react";

const TodoSort = ({ setTodos }) => {
  const [sortOrder, setSortOrder] = useState("asc");

  const onChangeSortOrder = (e) => {
    const nextSortOrder = e.target.value;

    setSortOrder(nextSortOrder);

    if (nextSortOrder === "asc") {
      // 오름차순 정렬
      setTodos((prev) =>
        [...prev].sort((a, b) => new Date(a.deadline) - new Date(b.deadline))
      );
      return;
    }
    //내림차순 정렬
    setTodos((prev) =>
      [...prev].sort((a, b) => new Date(b.deadline) - new Date(a.deadline))
    );
  };

  return (
    <div>
      <select value={sortOrder} onChange={onChangeSortOrder}>
        <option value="asc">오름차순</option>
        <option value="desc">내림차순</option>
      </select>
    </div>
  );
};
export default TodoSort;
```

<br><br>

# 2. useRef 사용하기

## 2.1 TodoForm.jsx

> TodoForm.jsx만 수정

```jsx
import { useEffect, useRef } from "react";
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = ({ setTodos }) => {
  const titleRef = useRef(null);
  const contentRef = useRef(null);
  const deadlineRef = useRef(null);

  useEffect(() => {
    // 컴포넌트가 마운트될 때 title 입력 필드에 포커스 설정
    titleRef.current.focus();
  }, []);

  const onSubmit = (e) => {
    e.preventDefault();

    const title = titleRef.current.value;
    const content = contentRef.current.value;
    const deadline = deadlineRef.current.value;

    if (!title || !content || !deadline) {
      alert("값을 입력하세요");
      return;
    }

    const addTodos = {
      id: uuid(),
      title,
      content,
      isDone: false,
      deadline,
    };

    setTodos((prev) => [...prev, addTodos]);

    // 입력폼 초기화
    titleRef.current.value = "";
    contentRef.current.value = "";
    deadlineRef.current.value = "";

    // 제출 후 첫 번째 입력 필드에 포커스 설정
    titleRef.current.focus();
  };

  return (
    <StTodoForm onSubmit={onSubmit}>
      <input type="text" name="title" ref={titleRef} placeholder="제목" />
      <input type="text" name="content" ref={contentRef} placeholder="내용" />
      <input type="date" name="deadline" ref={deadlineRef} />
      <button>추가</button>
    </StTodoForm>
  );
};

export default TodoForm;

const StTodoForm = styled.form`
  padding: 3rem;
`;
```

<br><br>

# 3. async와 await

## 3.1 세팅하기

### 3.1.1 JSON Server 실행

> `db.json` 파일 구성

```json
{
  "todos": [
    {
      "id": 1,
      "title": "제목1",
      "content": "내용1",
      "isDone": false,
      "deadline": "2024-07-09"
    },
    {
      "id": 2,
      "title": "제목2",
      "content": "내용2",
      "isDone": true,
      "deadline": "2024-06-16"
    }
  ]
}
```

<br>

> 그 후, JSON Server 설치 및 실행하기

```
yarn global add json-server
json-server --watch db.json --port 4000
```

<br>

### 3.1.2 .env

> React 프로젝트 루트 디렉토리에 `.env` 파일 생성 후, 환경변수 설정

```
REACT_APP_SERVER_URL=http://localhost:4000
```

<br>

이제 `${process.env.REACT_APP_SERVER_URL}/todos`와 같이 환경변수에 접근 가능!

⚠️ 개발 서버를 다시 시작하여 환경 변수 변경 사항을 반영해야 한다!

<br>

## 3.2 Home.jsx

```jsx
import axios from "axios";
import TodoForm from "components/TodoForm";
import TodoList from "components/TodoList";
import TodoSort from "components/TodoSort";
import { useEffect, useState } from "react";
import styled from "styled-components";

const Home = () => {
  const [todos, setTodos] = useState([]);

  // 조회 함수
  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const res = await axios.get(
          `${process.env.REACT_APP_SERVER_URL}/todos`
        );

        setTodos(res.data);
      } catch (error) {
        console.error("에러가 발생하였습니다.");
      }
    };
    fetchTodos();
  }, []);

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>

      <TodoForm setTodos={setTodos} />
      <TodoSort setTodos={setTodos} />

      <StH2>미완료 ({workingTodos.length})</StH2>
      <TodoList todos={workingTodos} setTodos={setTodos} />

      <StH2>할 일 완료 ({doneTodos.length})</StH2>
      <TodoList todos={doneTodos} setTodos={setTodos} />
    </StHomeLayout>
  );
};

export default Home;

const StHomeLayout = styled.main`
  display: flex;
  flex-direction: column;
  align-items: center;

  height: 100vh;
`;
const StH1 = styled.h1`
  font-size: 3rem;
  font-weight: bold;

  padding: 3rem;
`;

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

## 3.3 TodoForm.jsx

```jsx
import axios from "axios";
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = ({ setTodos }) => {
  const onSubmit = async (e) => {
    e.preventDefault();

    const title = e.target.title.value;
    const content = e.target.content.value;
    const deadline = e.target.deadline.value;

    if (!title || !content || !deadline) {
      alert("값을 입력하세요");
      return;
    }

    try {
      const res = await axios.post(
        `${process.env.REACT_APP_SERVER_URL}/todos`,
        {
          id: uuid(),
          title,
          content,
          isDone: false,
          deadline,
        }
      );
      setTodos((prev) => [...prev, res.data]);

      // 입력폼 초기화
      e.target.title.value = "";
      e.target.content.value = "";
      e.target.deadline.value = "";
    } catch (error) {
      console.error("추가 에러가 발생하였습니다.", error);
    }
  };

  return (
    <StTodoForm onSubmit={onSubmit}>
      <input type="text" name="title" placeholder="제목" />
      <input type="text" name="content" placeholder="내용" />
      <input type="date" name="deadline" />
      <button>추가</button>
    </StTodoForm>
  );
};

export default TodoForm;

const StTodoForm = styled.form`
  padding: 3rem;
`;
```

<br>

## 3.4 TodoItem.jsx

```jsx
import axios from "axios";
import { useState } from "react";
import styled from "styled-components";

const TodoItem = ({ todo, todos, setTodos }) => {
  const [edit, setEdit] = useState(null);

  // 할 일 삭제 함수
  const handleDeleteButton = async (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      try {
        await axios.delete(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`);

        setTodos((prev) => prev.filter((todo) => todo.id !== id));

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
      await axios.put(
        `${process.env.REACT_APP_SERVER_URL}/todos/${edit.id}`,
        edit
      );

      // 새로운 할 일 객체 생성
      const updatedTodo = {
        ...edit,
        title: edit.title,
        content: edit.content,
        deadline: edit.deadline,
      };

      // 이전 상태(prev)를 이용하여 새로운 할 일 목록 생성✨✨
      setTodos((prev) =>
        prev.map((todo) => (todo.id === edit.id ? updatedTodo : todo))
      );

      alert("수정 되셨습니다.");
      setEdit(null);
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  };

  // 할 일의 완료 상태 토글 함수
  const isDoneOnToggleButton = async (id) => {
    try {
      await axios.put(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`, {
        ...todo,
        isDone: !todo.isDone,
      });
      setTodos((prev) =>
        prev.map((todo) =>
          todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
        )
      );
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
            name="text"
            value={edit.title}
            onChange={(e) => setEdit({ ...edit, title: e.target.value })}
          />
          <input
            type="content"
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

<br>
