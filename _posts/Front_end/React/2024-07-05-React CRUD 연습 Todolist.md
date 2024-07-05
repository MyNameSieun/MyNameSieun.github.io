---
title: "[React] React CRUD 연습 Todolist"
categories: [React]
tag: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 등록 기능

```js
const handleAddButton = () => {
  if (!title || !content) {
    alert("값을 입력해주세요.");
    return;
  }
  const newTodos = {
    id: uuidv4(),
    title,
    content,
  };
  setTodos([...todos, newTodos]);
  setTitle("");
  setContent("");
};
```

<br><br>

# 2. 삭제 기능

```js
const handledeleteButton = (id) => {
  const isConfirmed = window.confirm("정말 삭제하시겠습니까?");
  if (isConfirmed) {
    const newTodos = todos.filter((todo) => todo.id !== id);
    setTodos(newTodos);
  }
};
```

<br><br>

# 3. 수정 기능

```js
const handleEditButton = (todo) => {
  setEditTodo(todo);
};

const handleUpdateButton = () => {
  const newTodos = todos.map((todo) =>
    todo.id === editTodo.id
      ? { ...todo, title: editTodo.title, content: editTodo.content }
      : todo
  );
  setTodos(newTodos);
  setEditTodo(null);
};
```

<br><br>

# 4. 전체 코드

```js
import { useState } from "react";
import styled from "styled-components";
import { v4 as uuidv4 } from "uuid";

const App = () => {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");
  const [todos, setTodos] = useState([]);
  const [editTodo, setEditTodo] = useState(null);

  const handleAddButton = () => {
    if (!title || !content) {
      alert("값을 입력해주세요.");
      return;
    }
    const newTodos = {
      id: uuidv4(),
      title,
      content,
    };
    setTodos([...todos, newTodos]);
    setTitle("");
    setContent("");
  };
  const handledeleteButton = (id) => {
    const isConfirmed = window.confirm("정말 삭제하시겠습니까?");
    if (isConfirmed) {
      const newTodos = todos.filter((todo) => todo.id !== id);
      setTodos(newTodos);
    }
  };
  const handleEditButton = (todo) => {
    setEditTodo(todo);
  };
  const handleUpdateButton = () => {
    // 최종적으로 편집이 완료된 todo 항목들을 업데이트하여 전체 todos 배열에 반영
    const newTodos = todos.map((todo) =>
      todo.id === editTodo.id
        ? { ...todo, title: editTodo.title, content: editTodo.content }
        : todo
    );
    setTodos(newTodos);
    setEditTodo(null);
  };

  return (
    <HomeLayout>
      <H1>Todolist</H1>
      <div>
        <input
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          placeholder="제목을 입력하세요."
        />
        <input
          value={content}
          onChange={(e) => setContent(e.target.value)}
          placeholder="내용을 입력하세요."
        />
        <button onClick={handleAddButton}>추가</button>
      </div>
      <div>
        {todos.map((todo) => (
          <TodoBox>
            {editTodo && editTodo.id === todo.id ? (
              <>
                <input
                  value={editTodo.title}
                  onChange={(e) =>
                    setEditTodo({ ...editTodo, title: e.target.value })
                  }
                  placeholder="수정할 제목을 입력하세요"
                />
                <input
                  value={editTodo.content}
                  onChange={(e) =>
                    setEditTodo({ ...editTodo, content: e.target.value })
                  }
                  placeholder="수정할 내용을 입력하세요"
                />
                <button onClick={() => handledeleteButton(todo.id)}>
                  삭제
                </button>
                <button onClick={handleUpdateButton}>완료</button>
              </>
            ) : (
              <>
                <p>제목: {todo.title}</p>
                <p>내용: {todo.content}</p>
                <button onClick={() => handledeleteButton(todo.id)}>
                  삭제
                </button>
                <button onClick={() => handleEditButton(todo)}>수정</button>
              </>
            )}
          </TodoBox>
        ))}
      </div>
    </HomeLayout>
  );
};

export default App;

const HomeLayout = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10rem;
`;

const H1 = styled.h1`
  font-size: 35px;
  font-weight: bold;
  margin-bottom: 20px;
`;

const TodoBox = styled.div`
  width: 300px;
  padding: 20px;
  margin-top: 20px;
  border: 1px solid #ccc;
`;
```

<br><br>

# 5. 수정기능 연습

> 아래 코드에서 수정 기능 추가해보기

```js
import { useState } from "react";
import styled from "styled-components";
import { v4 as uuidv4 } from "uuid";

const App = () => {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");
  const [todos, setTodos] = useState([]);
  const [editTodo, setEditTodo] = useState(null);

  const handleAddButton = () => {
    if (!title || !content) {
      alert("값을 입력해주세요.");
      return;
    }
    const newTodos = {
      id: uuidv4(),
      title,
      content,
    };
    setTodos([...todos, newTodos]);
    setTitle("");
    setContent("");
  };
  const handledeleteButton = (id) => {
    const isConfirmed = window.confirm("정말 삭제하시겠습니까?");
    if (isConfirmed) {
      const newTodos = todos.filter((todo) => todo.id !== id);
      setTodos(newTodos);
    }
  };
  const handleEditButton = () => {};
  const handleUpdateButton = () => {};

  return (
    <HomeLayout>
      <H1>Todolist</H1>
      <div>
        <input
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          placeholder="제목을 입력하세요."
        />
        <input
          value={content}
          onChange={(e) => setContent(e.target.value)}
          placeholder="내용을 입력하세요."
        />
        <button onClick={handleAddButton}>추가</button>
      </div>
      <div>
        {todos.map((todo) => (
          <TodoBox>
            <p>제목: {todo.title}</p>
            <p>내용: {todo.content}</p>
            <button onClick={() => handledeleteButton(todo.id)}>삭제</button>
          </TodoBox>
        ))}
      </div>
    </HomeLayout>
  );
};

export default App;

const HomeLayout = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10rem;
`;

const H1 = styled.h1`
  font-size: 35px;
  font-weight: bold;
  margin-bottom: 20px;
`;

const TodoBox = styled.div`
  width: 300px;
  padding: 20px;
  margin-top: 20px;
  border: 1px solid #ccc;
`;
```

<br>
