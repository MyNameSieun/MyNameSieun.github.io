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

> Home.jsx

```jsx
import TodoForm from "components/TodoForm";
import TodoList from "components/TodoList";
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

      <StH2>미완료</StH2>
      <TodoList todos={workingTodos} setTodos={setTodos} />

      <StH2>할 일 완료</StH2>
      <TodoList todos={doneTodos} setTodos={setTodos} />
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

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

> TodoForm.jsx

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

> TodoList.jsx

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

> TodoItem.jsx

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

<br><br>

## 1.2 useRef 사용하기

✨ Home.jsx, TodoList.jsx, TodoForm.jsx는 동일

> TodoForm.jsx

```jsx
import { useEffect, useRef } from "react";
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = ({ setTodos }) => {
  useEffect(() => {
    // 컴포넌트가 마운트될 때 title 입력 필드에 포커스 설정
    titleRef.current.focus();
  }, []);

  const titleRef = useRef(null);
  const contentRef = useRef(null);
  const deadlineRef = useRef(null);

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

<br>

---

<br>

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
