---
title: "[React] 5가지 방법으로 Todolist 만들기(ReactHooks, RestApi)"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. React Hooks

## 1.1 state 사용하기

```jsx
import styled from "styled-components";
import { useState } from "react";
import { v4 as uuid } from "uuid";

const Home = () => {
  const [title, setTitle] = useState("");
  const [contents, setContents] = useState("");
  const [todos, setTodos] = useState([]);
  const [isEdit, setIsEdit] = useState(null);

  const handleOnsubmit = (e) => {
    e.preventDefault();

    if (!title || !contents) {
      alert("내용을 입력하세요");
      return;
    }

    const addTodos = {
      id: uuid(),
      title,
      contents,
      isDone: false,
    };

    setTodos([...todos, addTodos]);
    setTitle("");
    setContents("");
  };
  const handleDeleteButton = (id) => {
    const isConfirmed = window.confirm("정말 삭제하시겠습니까?");
    if (isConfirmed) {
      const newTodos = todos.filter((todo) => todo.id !== id);
      setTodos(newTodos);
      alert("삭제되었습니다.");
    }
  };

  const handleUpdateButton = () => {
    // 최종적으로 편집이 완료된 todo 항목들을 업데이트하여 전체 todos 배열에 반영
    const newTodos = todos.map((todo) =>
      todo.id === isEdit.id
        ? { ...todo, title: isEdit.title, contents: isEdit.contents }
        : todo
    );
    setTodos(newTodos);
    setIsEdit(null);
  };

  const handleEditButton = (todo) => {
    setIsEdit(todo);
  };

  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>

      <StTodoForm onSubmit={handleOnsubmit}>
        <input
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          placeholder="제목을 입력하세요"
        />
        <input
          value={contents}
          onChange={(e) => setContents(e.target.value)}
          placeholder="내용을 입력하세요"
        />
        <button>추가하기</button>
      </StTodoForm>

      {todos.map((todo) => (
        <StTodoBox key={todo.id}>
          {isEdit && isEdit.id == todo.id ? (
            <>
              <input
                value={isEdit.title}
                onChange={(e) =>
                  setIsEdit({ ...isEdit, title: e.target.value })
                }
              />
              <input
                value={isEdit.contents}
                onChange={(e) =>
                  setIsEdit({ ...isEdit, contents: e.target.value })
                }
              />
              <button onClick={() => handleDeleteButton(todo.id)}>
                삭제하기
              </button>
              <button onClick={handleUpdateButton}>수정 완료</button>
            </>
          ) : (
            <>
              <div>제목: {todo.title}</div>
              <div>내용: {todo.contents}</div>
              <button onClick={() => handleDeleteButton(todo.id)}>
                삭제하기
              </button>
              <button onClick={() => handleEditButton(todo)}>수정하기</button>
            </>
          )}
        </StTodoBox>
      ))}
    </StHomeLayout>
  );
};

export default Home;

const StHomeLayout = styled.div`
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

const StTodoForm = styled.form`
  padding: 3rem;
`;

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

## 1.2 useRef 사용하기

<br><br>

# 2. REST API를 활용하기

## 2.0 JSON Server 설정하기

> JSON Server를 설치하여 간단히 데이터베이스를 설정

```json
{
  "todos": [
    { "id": 1, "title": "할 일 1", "content": "내용 1", "isDone": false },
    { "id": 2, "title": "할 일 2", "content": "내용 2", "isDone": true }
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

## 2.1 fetch 사용하기

> TodoList 컴포넌트 (Todo 리스트 조회)

```js
import React, { useEffect, useState } from "react";
import TodoItem from "./TodoItem";

const TodoList = () => {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3001/todos")
      .then((res) => res.json())
      .then((todos) => setTodos(todos))
      .catch((error) => console.log("Error fetching todos:", error));
  }, []);

  return (
    <ul>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} setTodos={setTodos} />
      ))}
    </ul>
  );
};

export default TodoList;
```

<br>

> TodoForm 컴포넌트 (Todo 추가)

```js
import { useEffect, useRef } from "react";
import { useNavigate } from "react-router-dom";
import styled from "styled-components";

const TodoForm = () => {
  const history = useNavigate();

  const titleRef = useRef(null);
  const contentRef = useRef(null);

  useEffect(() => {
    titleRef.current.focus();
  }, []);

  const onSubmit = (e) => {
    e.preventDefault();

    fetch(`http://localhost:3001/todos`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title: titleRef.current.value,
        content: contentRef.current.value,
        isDone: false,
      }),
    })
      .then((res) => {
        if (!titleRef.current.value || !contentRef.current.value) {
          alert("값을 입력해주세요.");
          return;
        }
        if (res.ok) {
          alert("생성이 완료되었습니다!");
          history("/");
        }
      })
      .catch((err) => {
        console.error("Error", err);
      });
  };

  return (
    <form onSubmit={onSubmit}>
      <p>
        제목: <input type="text" ref={titleRef} />
      </p>
      <p>
        내용: <input type="text" ref={contentRef} />
      </p>
      <button>추가</button>
    </form>
  );
};

export default TodoForm;
```

<br>

> TodoItem 컴포넌트 (Todo 수정, 삭제, 완료 상태 변경)

```js
import { useState } from "react";
import styled from "styled-components";

const TodoItem = ({ todo, setTodos }) => {
  const [isDone, setIsDone] = useState(todo.isDone);
  const [isEditing, setIsEditing] = useState(false);
  const [title, setTitle] = useState(todo.title);
  const [content, setContent] = useState(todo.content);

  const handleDeleteButton = (id) => {
    if (window.confirm("삭제하시겠습니까?")) {
      fetch(`http://localhost:3001/todos/${id}`, {
        method: "DELETE",
      }).then((res) => {
        if (res.ok) {
          alert("삭제되었습니다");
          setTodos((prev) => prev.filter((todo) => todo.id !== id));
        }
      });
    }
  };

  const handleDoneToggleButton = () => {
    fetch(`http://localhost:3001/todos/${todo.id}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        ...todo,
        isDone: !isDone,
      }),
    })
      .then((res) => {
        if (res.ok) {
          setIsDone(!isDone);
        }
      })
      .catch((err) => {
        console.error("Error updating todo:", err);
      });
  };

  const handleUpdateButton = () => {
    fetch(`http://localhost:3001/todos/${todo.id}`, {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        ...todo,
        title: title,
        content: content,
      }),
    })
      .then((res) => {
        if (res.ok) {
          alert("수정되었습니다");
          setTodos((prev) =>
            prev.map((item) =>
              item.id === todo.id
                ? { ...item, title: title, content: content }
                : item
            )
          );
          setIsEditing(false);
        }
      })
      .catch((err) => {
        console.error("Error updating todo:", err);
      });
  };

  const handleEditButton = () => {
    setIsEditing(true);
  };

  return (
    <StTodoItemBox isDone={isDone}>
      {isEditing ? (
        <>
          <li>
            제목:
            <input value={title} onChange={(e) => setTitle(e.target.value)} />
          </li>
          <li>
            내용:
            <input
              value={content}
              onChange={(e) => setContent(e.target.value)}
            />
          </li>
          <button onClick={() => handleDeleteButton(todo.id)}>삭제</button>
          <button onClick={handleDoneToggleButton}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleUpdateButton}>수정 완료</button>
        </>
      ) : (
        <>
          <li>제목: {todo.title}</li>
          <li>내용: {todo.content}</li>
          <button onClick={() => handleDeleteButton(todo.id)}>삭제</button>
          <button onClick={handleDoneToggleButton}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleEditButton}>글 수정하기</button>
        </>
      )}
    </StTodoItemBox>
  );
};

export default TodoItem;

const StTodoItemBox = styled.div`
  border: 1px solid black;
  padding: 2rem;
  background-color: ${(props) => (props.isDone ? "#cccccc" : "#ffffff")};
`;
```

<br>

## 2.2 async, await 사용하기

> TodoList 컴포넌트 (Todo 리스트 조회)

```js
import React, { useEffect, useState } from "react";
import TodoItem from "./TodoItem";

const TodoList = () => {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await fetch("http://localhost:3001/todos");
        if (!response.ok) {
          throw new Error("서버에서 Todo 리스트를 불러오는데 실패했습니다.");
        }
        const data = await response.json();
        setTodos(data);
      } catch (error) {
        console.error("Todo 리스트를 불러오는 중 오류 발생:", error);
      }
    };

    fetchTodos();
  }, []);

  return (
    <ul>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} setTodos={setTodos} />
      ))}
    </ul>
  );
};

export default TodoList;
```

<br>

> TodoForm 컴포넌트 (Todo 추가)

```js
import { useEffect, useRef } from "react";
import { useNavigate } from "react-router-dom";

const TodoForm = () => {
  const navigate = useNavigate();
  const titleRef = useRef(null);
  const contentRef = useRef(null);

  useEffect(() => {
    titleRef.current.focus();
  }, []);

  const onSubmit = async (e) => {
    e.preventDefault();

    const title = titleRef.current.value.trim();
    const content = contentRef.current.value.trim();

    if (!title || !content) {
      alert("제목과 내용을 입력해주세요.");
      return;
    }

    try {
      const response = await fetch(`http://localhost:3001/todos`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          title,
          content,
          isDone: false,
        }),
      });

      if (!response.ok) {
        throw new Error("Todo 추가에 실패했습니다.");
      }

      alert("Todo가 추가되었습니다.");
      navigate("/");
    } catch (error) {
      console.error("Todo 추가 중 오류 발생:", error);
    }
  };

  return (
    <form onSubmit={onSubmit}>
      <p>
        제목: <input type="text" ref={titleRef} />
      </p>
      <p>
        내용: <input type="text" ref={contentRef} />
      </p>
      <button type="submit">추가</button>
    </form>
  );
};

export default TodoForm;
```

<br>

> TodoItem 컴포넌트 (Todo 수정, 삭제, 완료 상태 변경)

```js
import { useState } from "react";

const TodoItem = ({ todo, setTodos }) => {
  const [isDone, setIsDone] = useState(todo.isDone);
  const [isEditing, setIsEditing] = useState(false);
  const [title, setTitle] = useState(todo.title);
  const [content, setContent] = useState(todo.content);

  const handleDelete = async (id) => {
    if (window.confirm("정말 삭제하시겠습니까?")) {
      try {
        const response = await fetch(`http://localhost:3001/todos/${id}`, {
          method: "DELETE",
        });

        if (!response.ok) {
          throw new Error("Todo 삭제에 실패했습니다.");
        }

        alert("Todo가 삭제되었습니다.");
        setTodos((prevTodos) => prevTodos.filter((item) => item.id !== id));
      } catch (error) {
        console.error("Todo 삭제 중 오류 발생:", error);
      }
    }
  };

  const handleToggleDone = async () => {
    try {
      const response = await fetch(`http://localhost:3001/todos/${todo.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          ...todo,
          isDone: !isDone,
        }),
      });

      if (!response.ok) {
        throw new Error("Todo 상태 변경에 실패했습니다.");
      }

      setIsDone(!isDone);
    } catch (error) {
      console.error("Todo 상태 변경 중 오류 발생:", error);
    }
  };

  const handleUpdate = async () => {
    try {
      const response = await fetch(`http://localhost:3001/todos/${todo.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          ...todo,
          title,
          content,
        }),
      });

      if (!response.ok) {
        throw new Error("Todo 수정에 실패했습니다.");
      }

      alert("Todo가 수정되었습니다.");
      setTodos((prevTodos) =>
        prevTodos.map((item) =>
          item.id === todo.id ? { ...item, title, content } : item
        )
      );
      setIsEditing(false);
    } catch (error) {
      console.error("Todo 수정 중 오류 발생:", error);
    }
  };

  const handleEdit = () => {
    setIsEditing(true);
  };

  return (
    <div>
      {isEditing ? (
        <div>
          <input value={title} onChange={(e) => setTitle(e.target.value)} />
          <input value={content} onChange={(e) => setContent(e.target.value)} />
          <button onClick={() => handleDelete(todo.id)}>삭제</button>
          <button onClick={handleToggleDone}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleUpdate}>수정 완료</button>
        </div>
      ) : (
        <div>
          <div>{todo.title}</div>
          <div>{todo.content}</div>
          <button onClick={() => handleDelete(todo.id)}>삭제</button>
          <button onClick={handleToggleDone}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleEdit}>수정하기</button>
        </div>
      )}
    </div>
  );
};

export default TodoItem;
```

<br>

## 2.3 axios 사용하기

> TodoList 컴포넌트

```jsx
import React, { useEffect, useState } from "react";
import axios from "axios";
import TodoItem from "./TodoItem";

const TodoList = () => {
  const [todos, setTodos] = useState([]);

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const response = await axios.get("http://localhost:3001/todos");
        setTodos(response.data);
      } catch (error) {
        console.error("Todo 리스트를 불러오는 중 오류 발생:", error);
      }
    };

    fetchTodos();
  }, []);

  return (
    <ul>
      {todos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} setTodos={setTodos} />
      ))}
    </ul>
  );
};

export default TodoList;
```

<br>

> TodoForm 컴포넌트

```jsx
import React, { useEffect, useRef } from "react";
import { useNavigate } from "react-router-dom";
import axios from "axios";

const TodoForm = () => {
  const navigate = useNavigate();
  const titleRef = useRef(null);
  const contentRef = useRef(null);

  useEffect(() => {
    titleRef.current.focus();
  }, []);

  const onSubmit = async (e) => {
    e.preventDefault();

    const title = titleRef.current.value.trim();
    const content = contentRef.current.value.trim();

    if (!title || !content) {
      alert("제목과 내용을 입력해주세요.");
      return;
    }

    try {
      await axios.post("http://localhost:3001/todos", {
        title,
        content,
        isDone: false,
      });

      alert("Todo가 추가되었습니다.");
      navigate("/");
    } catch (error) {
      console.error("Todo 추가 중 오류 발생:", error);
    }
  };

  return (
    <form onSubmit={onSubmit}>
      <p>
        제목: <input type="text" ref={titleRef} />
      </p>
      <p>
        내용: <input type="text" ref={contentRef} />
      </p>
      <button type="submit">추가</button>
    </form>
  );
};

export default TodoForm;
```

<br>

> TodoItem 컴포넌트

```jsx
import React, { useState } from "react";
import axios from "axios";

const TodoItem = ({ todo, setTodos }) => {
  const [isDone, setIsDone] = useState(todo.isDone);
  const [isEditing, setIsEditing] = useState(false);
  const [title, setTitle] = useState(todo.title);
  const [content, setContent] = useState(todo.content);

  const handleDelete = async (id) => {
    if (window.confirm("정말 삭제하시겠습니까?")) {
      try {
        await axios.delete(`http://localhost:3001/todos/${id}`);
        alert("Todo가 삭제되었습니다.");
        setTodos((prevTodos) => prevTodos.filter((item) => item.id !== id));
      } catch (error) {
        console.error("Todo 삭제 중 오류 발생:", error);
      }
    }
  };

  const handleToggleDone = async () => {
    try {
      await axios.put(`http://localhost:3001/todos/${todo.id}`, {
        ...todo,
        isDone: !isDone,
      });

      setIsDone(!isDone);
    } catch (error) {
      console.error("Todo 상태 변경 중 오류 발생:", error);
    }
  };

  const handleUpdate = async () => {
    try {
      await axios.put(`http://localhost:3001/todos/${todo.id}`, {
        ...todo,
        title,
        content,
      });

      alert("Todo가 수정되었습니다.");
      setTodos((prevTodos) =>
        prevTodos.map((item) =>
          item.id === todo.id ? { ...item, title, content } : item
        )
      );
      setIsEditing(false);
    } catch (error) {
      console.error("Todo 수정 중 오류 발생:", error);
    }
  };

  const handleEdit = () => {
    setIsEditing(true);
  };

  return (
    <div>
      {isEditing ? (
        <div>
          <input value={title} onChange={(e) => setTitle(e.target.value)} />
          <input value={content} onChange={(e) => setContent(e.target.value)} />
          <button onClick={() => handleDelete(todo.id)}>삭제</button>
          <button onClick={handleToggleDone}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleUpdate}>수정 완료</button>
        </div>
      ) : (
        <div>
          <div>{todo.title}</div>
          <div>{todo.content}</div>
          <button onClick={() => handleDelete(todo.id)}>삭제</button>
          <button onClick={handleToggleDone}>
            {isDone ? "할 일 취소" : "할 일 완료"}
          </button>
          <button onClick={handleEdit}>수정하기</button>
        </div>
      )}
    </div>
  );
};

export default TodoItem;
```

<br>
