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
        <TodoItem key={todo.id} todo={todo} todos={todos} setTodos={setTodos} />
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

const TodoItem = ({ todo, todos, setTodos }) => {
  const [edit, setEdit] = useState(null);

  const handleDeleteButton = (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      const newTodos = todos.filter((todo) => todo.id !== id);
      setTodos(newTodos);
      alert("삭제 되셨습니다.");
    }
  };

  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  const handleUpdateButton = () => {
    const newTodos = todos.map((todo) =>
      todo.id === edit.id
        ? {
            ...todo,
            title: edit.title,
            content: edit.content,
            deadline: edit.deadline,
          }
        : todo
    );
    setTodos(newTodos);
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
            name="text"
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

## 3.1 JSON Server 설정하기

> JSON Server를 설치하여 간단히 데이터베이스를 설정

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

> db설정 후, JSON Server 설치 및 실행하기

```
yarn global add json-server
json-server --watch db.json --port 3001
```

<br>

## 3.2 Home.jsx

```jsx
import TodoForm from "components/TodoForm";
import TodoList from "components/TodoList";
import TodoSort from "components/TodoSort";
import { useState, useEffect } from "react";
import styled from "styled-components";
import axios from "axios";

const Home = () => {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await axios.get("http://localhost:4000/todos"); // 서버에서 todos 가져오기
        setTodos(response.data); // 가져온 데이터로 todos 상태 업데이트
      } catch (error) {
        console.error("Error fetching todos:", error);
      }
    };

    fetchTodos();
  }, []); // 컴포넌트가 처음 렌더링될 때 한 번만 실행되도록 빈 배열을 useEffect의 두 번째 인자로 전달

  const updateTodos = async (updatedTodos) => {
    try {
      await axios.put("http://localhost:4000/todos", updatedTodos); // 서버에 todos 업데이트
      setTodos(updatedTodos); // 업데이트된 todos 상태로 업데이트
    } catch (error) {
      console.error("Error updating todos:", error);
    }
  };

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>
      <TodoForm setTodos={updateTodos} />
      <TodoSort setTodos={setTodos} />

      <StH2>미완료</StH2>
      <TodoList todos={workingTodos} setTodos={updateTodos} />

      <StH2>할 일 완료</StH2>
      <TodoList todos={doneTodos} setTodos={updateTodos} />
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
import styled from "styled-components";
import { v4 as uuid } from "uuid";
import axios from "axios"; // axios 추가

const TodoForm = ({ setTodos }) => {
  const onSubmit = async (e) => {
    // 비동기 함수로 변경
    e.preventDefault();

    const title = e.target.title.value;
    const content = e.target.content.value;
    const deadline = e.target.deadline.value;

    if (!title || !content || !deadline) {
      alert("값을 입력하세요");
      return;
    }

    const addTodo = {
      id: uuid(),
      title,
      content,
      isDone: false,
      deadline,
    };

    try {
      // POST 요청을 보내어 서버에 새로운 할 일 추가
      const response = await axios.post("http://localhost:4000/todos", addTodo);

      // 성공적으로 추가되면 서버에서 반환한 데이터를 기존 할 일 목록에 추가
      setTodos((prev) => [...prev, response.data]);

      // 입력폼 초기화
      e.target.title.value = "";
      e.target.content.value = "";
      e.target.deadline.value = "";
    } catch (error) {
      console.error("Error adding todo:", error);
      alert("할 일 추가에 실패했습니다.");
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
import { useState } from "react";
import styled from "styled-components";
import axios from "axios"; // axios 추가

const TodoItem = ({ todo, todos, setTodos }) => {
  const [edit, setEdit] = useState(null);

  const handleDeleteButton = async (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      try {
        await axios.delete(`http://localhost:4000/todos/${id}`);
        const newTodos = todos.filter((todo) => todo.id !== id);
        setTodos(newTodos);
        alert("삭제 되셨습니다.");
      } catch (error) {
        console.error("Error deleting todo:", error);
        alert("할 일 삭제에 실패했습니다.");
      }
    }
  };

  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  const handleUpdateButton = async () => {
    try {
      await axios.put(`http://localhost:4000/todos/${edit.id}`, edit);
      const newTodos = todos.map((t) => (t.id === edit.id ? edit : t));
      setTodos(newTodos);
      alert("수정 되셨습니다.");
      setEdit(null);
    } catch (error) {
      console.error("Error updating todo:", error);
      alert("할 일 수정에 실패했습니다.");
    }
  };

  const isDoneOnToggleButton = async (id) => {
    try {
      // 서버에 PUT 요청을 보냄
      const updatedTodo = await axios.put(`http://localhost:4000/todos/${id}`, {
        ...todo,
        isDone: !todo.isDone,
      });

      // 서버에서 반환된 업데이트된 할 일 객체를 받아와서 todos 상태를 업데이트
      const updatedTodos = todos.map((t) =>
        t.id === id ? updatedTodo.data : t
      );
      setTodos(updatedTodos);
    } catch (error) {
      console.error("할 일 상태 업데이트 오류:", error);
      alert("할 일 상태 변경에 실패했습니다.");
    }
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

## 3.5 TodoList.jsx

```jsx
import { useState, useEffect } from "react";
import axios from "axios";
import TodoItem from "./TodoItem";

const TodoList = ({ setTodos }) => {
  const [todos, setTodosState] = useState([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await axios.get("http://localhost:4000/todos"); // 서버에서 todos 가져오기
        setTodosState(response.data); // 가져온 데이터로 todos 상태 업데이트
      } catch (error) {
        console.error("Error fetching todos:", error);
      }
    };

    fetchTodos();
  }, []); // 컴포넌트가 처음 렌더링될 때 한 번만 실행되도록 빈 배열을 useEffect의 두 번째 인자로 전달

  return (
    <>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} todos={todos} setTodos={setTodos} />
      ))}
    </>
  );
};

export default TodoList;
```

<br>

## 3.5 TodoSort.jsx

```jsx
import { useState, useEffect } from "react";
import axios from "axios";

const TodoSort = ({ setTodos }) => {
  const [sortOrder, setSortOrder] = useState("asc");
  const [localTodos, setLocalTodos] = useState([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await axios.get("http://localhost:4000/todos");
        setLocalTodos(response.data); // 서버에서 받은 데이터를 localTodos 상태에 저장
      } catch (error) {
        console.error("Error fetching todos:", error);
      }
    };

    fetchTodos();
  }, []); // 컴포넌트가 처음 렌더링될 때 한 번만 실행

  const onChangeSortOrder = (e) => {
    const nextSortOrder = e.target.value;
    setSortOrder(nextSortOrder);

    if (nextSortOrder === "asc") {
      // 오름차순 정렬
      setLocalTodos((prev) =>
        [...prev].sort((a, b) => new Date(a.deadline) - new Date(b.deadline))
      );
    } else {
      // 내림차순 정렬
      setLocalTodos((prev) =>
        [...prev].sort((a, b) => new Date(b.deadline) - new Date(a.deadline))
      );
    }
  };

  useEffect(() => {
    // 정렬된 localTodos를 전체 todos에 반영
    setTodos(localTodos);
  }, [localTodos, setTodos]);

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

<br>
