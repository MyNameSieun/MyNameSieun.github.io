---
title: "[React] Context API, Redux, Zustand, Jotai로 Todolist 전역상태 관리하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

[[state, useRef, axios를 활용한 Todolist 만들기 - chap3:axios↗️]](https://mynamesieun.github.io/react/state,-useRef,-axios%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Todolist-%EB%A7%8C%EB%93%A4%EA%B8%B0/)에 이어 작성하는 글입니다.
{: .notice--danger}

<br>

# 1. Context API

## 1.1 TodoContext.jsx

> src > context > TodoContext.jsx 생성

{% raw %}

```jsx
import { createContext, useState, useEffect } from "react";
import { v4 as uuid } from "uuid";
import axios from "axios";

export const TodoContext = createContext();

export const TodoContextProvider = ({ children }) => {
  const [todos, setTodos] = useState([]);

  // 초기 할 일 데이터 가져오기
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

  // 할 일 수정
  const updateTodo = async (updatedTodo) => {
    try {
      await axios.put(
        `${process.env.REACT_APP_SERVER_URL}/todos/${updatedTodo.id}`,
        updatedTodo
      );
      setTodos((prevTodos) =>
        prevTodos.map((todo) =>
          todo.id === updatedTodo.id ? updatedTodo : todo
        )
      );
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  };

  // 할 일 정렬
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
        updateTodo,
        sortTodos,
      }}
    >
      {children}
    </TodoContext.Provider>
  );
};
```

{% endraw %}

<br>

## 1.2 Home.jsx

```jsx
import TodoForm from "../TodoForm";
import TodoList from "../TodoList";
import TodoSort from "../TodoSort";
import styled from "styled-components";
import { TodoContextProvider } from "context/TodoContext";

const Home = () => {
  return (
    <TodoContextProvider>
      <StHomeLayout>
        <StH1>TodoList</StH1>

        <TodoForm />
        <TodoSort />
        <TodoList />
      </StHomeLayout>
    </TodoContextProvider>
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
```

<br>

## 1.3 TodoForm.jsx

```jsx
import axios from "axios";
import { TodoContext } from "context/TodoContext";
import { useContext } from "react";
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = () => {
  const { addTodo } = useContext(TodoContext);

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
      addTodo(res.data);

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

## 1.4 TodoList.jsx

```jsx
import { useContext } from "react";
import TodoItem from "./TodoItem";
import { TodoContext } from "context/TodoContext";
import styled from "styled-components";

const TodoList = () => {
  const { todos } = useContext(TodoContext);

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <>
      <StH2>미완료</StH2>
      {workingTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
      <StH2>할 일 완료</StH2>
      {doneTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </>
  );
};

export default TodoList;

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

## 1.5 TodoItem.jsx

```jsx
import axios from "axios";
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
        await axios.delete(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`);
        deleteTodo(id);
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
  const handleUpdateButton = () => {
    updateTodo(edit);
    alert("수정되었습니다.");
    setEdit(null);
  };

  // 할 일의 완료 상태 토글 함수
  const isDoneOnToggleButton = async (id) => {
    try {
      await axios.put(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`, {
        ...todo,
        isDone: !todo.isDone,
      });
      toggleTodo(id);
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

## 1.6 TodoSort.jsx

```jsx
import { TodoContext } from "context/TodoContext";
import { useContext } from "react";

const TodoSort = () => {
  const { sortTodos } = useContext(TodoContext);

  const onChangeSortOrder = (e) => {
    const nextSortOrder = e.target.value;
    sortTodos(nextSortOrder);
  };

  return (
    <div>
      <select onChange={onChangeSortOrder}>
        <option value="asc">오름차순</option>
        <option value="desc">내림차순</option>
      </select>
    </div>
  );
};

export default TodoSort;
```

<br><br>

# 2. Redux

## 2.0 파일 구조

<br>

## 2.1 index.js

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import { Provider } from "react-redux";
import store from "./redux/config/configStore";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

reportWebVitals();
```

<br>

## 2.2 todos.js - 모듈 생성

```jsx

```

<br>

## 2.3 configStore.js - 리듀서 연결

```jsx
import { createStore } from "redux";
import { combineReducers } from "redux";
import todos from "../modules/todos";

const rootReducer = combineReducers({
  todos,
});

const store = createStore(rootReducer);

export default store;
```

## 2.5

<br>

## 2.6

<br>

## 2.7

<br>

## 2.8

<br>

<br><br>

# 3. Zustand

> - 베이스 코드 : Context API
> - `Provider` 가 존재하지 않는다.

## 3.1 Zustand 설치

```
yarn add zustand
```

<br>

## 3.2 src/store/useTodos.jsx

```jsx
import create from "zustand";
import { v4 as uuid } from "uuid";
import axios from "axios";

const useTodoStore = create((set) => ({
  todos: [],
  setTodos: (todos) => set({ todos }),
  fetchTodos: async () => {
    try {
      const res = await axios.get(`${process.env.REACT_APP_SERVER_URL}/todos`);
      set({ todos: res.data });
    } catch (error) {
      console.error("에러가 발생하였습니다.");
    }
  },
  addTodo: (newTodo) =>
    set((state) => ({
      todos: [...state.todos, { ...newTodo, id: uuid(), isDone: false }],
    })),
  deleteTodo: (id) =>
    set((state) => ({
      todos: state.todos.filter((todo) => todo.id !== id),
    })),
  toggleTodo: (id) =>
    set((state) => ({
      todos: state.todos.map((todo) =>
        todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
      ),
    })),
  updateTodo: async (updatedTodo) => {
    try {
      await axios.put(
        `${process.env.REACT_APP_SERVER_URL}/todos/${updatedTodo.id}`,
        updatedTodo
      );
      set((state) => ({
        todos: state.todos.map((todo) =>
          todo.id === updatedTodo.id ? updatedTodo : todo
        ),
      }));
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  },
  sortTodos: (order) => {
    set((state) => {
      const sortedTodos = [...state.todos].sort((a, b) => {
        return order === "asc"
          ? new Date(a.deadline) - new Date(b.deadline)
          : new Date(b.deadline) - new Date(a.deadline);
      });
      return { todos: sortedTodos };
    });
  },
}));

export default useTodoStore;
```

<br>

## 3.3 Home.jsx

```jsx
import TodoForm from "../TodoForm";
import TodoList from "../TodoList";
import TodoSort from "../TodoSort";
import styled from "styled-components";

const Home = () => {
  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>
      <TodoForm />
      <TodoSort />
      <TodoList />
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
```

<br>

## 3.4 TodoForm.jsx

```jsx
import axios from "axios";
import useTodoStore from "../store/useTodos";
import styled from "styled-components";
import { v4 as uuid } from "uuid";

const TodoForm = () => {
  /* 1. 구조 분해 할당으로 가져오기
  const { addTodo } = useTodoStore(); */

  // 2. 상태 선택 함수로 가져오기
  const addTodo = useTodoStore((state) => state.addTodo);

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
      addTodo(res.data);

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

## 3.5 TodoItem.jsx

```jsx
import axios from "axios";
import useTodoStore from "../store/useTodos";
import { useState } from "react";
import styled from "styled-components";

const TodoItem = ({ todo }) => {
  const deleteTodo = useTodoStore((state) => state.deleteTodo);
  const toggleTodo = useTodoStore((state) => state.toggleTodo);
  const updateTodo = useTodoStore((state) => state.updateTodo);

  const [edit, setEdit] = useState(null);

  const handleDeleteButton = async (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      try {
        await axios.delete(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`);
        deleteTodo(id); // 서버 요청이 성공하면 상태 업데이트
        alert("삭제되었습니다.");
      } catch (error) {
        console.error("할 일을 삭제하는 중 오류가 발생했습니다:", error);
      }
    }
  };

  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  const handleUpdateButton = () => {
    updateTodo(edit);
    alert("수정되었습니다.");
    setEdit(null);
  };

  const isDoneOnToggleButton = async (id) => {
    try {
      await axios.put(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`, {
        ...todo,
        isDone: !todo.isDone,
      });
      toggleTodo(id);
    } catch (error) {
      console.error(
        "할 일의 완료 상태를 토글하는 중 오류가 발생했습니다:",
        error
      );
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

## 3.6 TodoList.jsx

```jsx
import useTodoStore from "../store/useTodos";
import TodoItem from "./TodoItem";
import styled from "styled-components";
import { useEffect } from "react";

const TodoList = () => {
  const todos = useTodoStore((state) => state.todos);
  const fetchTodos = useTodoStore((state) => state.fetchTodos);

  useEffect(() => {
    fetchTodos();
  }, [fetchTodos]);

  if (!Array.isArray(todos)) {
    return <div>Loading...</div>;
  }

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <>
      <StH2>미완료</StH2>
      {workingTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
      <StH2>할 일 완료</StH2>
      {doneTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </>
  );
};

export default TodoList;

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

## 3.7 TodoSort.jsx

```jsx
import useTodoStore from "../store/useTodos";

const TodoSort = () => {
  const sortTodos = useTodoStore((state) => state.sortTodos);

  const onChangeSortOrder = (e) => {
    const nextSortOrder = e.target.value;
    sortTodos(nextSortOrder);
  };

  return (
    <div>
      <select onChange={onChangeSortOrder}>
        <option value="asc">오름차순</option>
        <option value="desc">내림차순</option>
      </select>
    </div>
  );
};

export default TodoSort;
```

<br><br>

# 4. Jotai

> 베이스 코드 : Context API

## 4.1 jotai 설치

```
yarn add jotai
```

<br>

## 4.2 App.jsx

기본적으로 Jotai는 전역 상태를 사용하지만, Provider를 통해 상태를 특정 범위에서만 사용할 수 있다.

```jsx
import Router from "./shared/Router";
import { Provider } from "jotai";

function App() {
  return (
    <Provider>
      <Router />
    </Provider>
  );
}

export default App;
```

<br>

## 4.3 src/store/todoAtom.js

```jsx
import { atom, useAtom } from "jotai";
import { useEffect } from "react";
import axios from "axios";
import { v4 as uuid } from "uuid";

// Atoms
const todosAtom = atom([]);

// Actions
const addTodoAtom = atom(
  (get) => get(todosAtom),
  (get, set, newTodo) => {
    set(todosAtom, [
      ...get(todosAtom),
      { ...newTodo, id: uuid(), isDone: false },
    ]);
  }
);

const deleteTodoAtom = atom(
  (get) => get(todosAtom),
  (get, set, id) => {
    set(
      todosAtom,
      get(todosAtom).filter((todo) => todo.id !== id)
    );
  }
);

const toggleTodoAtom = atom(
  (get) => get(todosAtom),
  (get, set, id) => {
    set(
      todosAtom,
      get(todosAtom).map((todo) =>
        todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
      )
    );
  }
);

const updateTodoAtom = atom(
  (get) => get(todosAtom),
  async (get, set, updatedTodo) => {
    try {
      await axios.put(
        `${process.env.REACT_APP_SERVER_URL}/todos/${updatedTodo.id}`,
        updatedTodo
      );
      set(
        todosAtom,
        get(todosAtom).map((todo) =>
          todo.id === updatedTodo.id ? updatedTodo : todo
        )
      );
    } catch (error) {
      console.error("할 일을 수정하는 중 오류가 발생했습니다:", error);
    }
  }
);

const sortTodosAtom = atom(
  (get) => get(todosAtom),
  (get, set, order) => {
    if (order === "asc") {
      set(
        todosAtom,
        [...get(todosAtom)].sort(
          (a, b) => new Date(a.deadline) - new Date(b.deadline)
        )
      );
    } else if (order === "desc") {
      set(
        todosAtom,
        [...get(todosAtom)].sort(
          (a, b) => new Date(b.deadline) - new Date(a.deadline)
        )
      );
    }
  }
);

// Fetch initial todos
const useFetchTodos = () => {
  const [, setTodos] = useAtom(todosAtom);

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
  }, [setTodos]);
};

export {
  todosAtom,
  addTodoAtom,
  deleteTodoAtom,
  toggleTodoAtom,
  updateTodoAtom,
  sortTodosAtom,
  useFetchTodos,
};
```

<br>

## 4.4 Home.jsx

useFetchTodos 훅을 사용

```jsx
import TodoForm from "../TodoForm";
import TodoList from "../TodoList";
import TodoSort from "../TodoSort";
import styled from "styled-components";
import { useFetchTodos } from "store/todoAtom";

const Home = () => {
  useFetchTodos();

  return (
    <StHomeLayout>
      <StH1>TodoList</StH1>
      <TodoForm />
      <TodoSort />
      <TodoList />
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
```

<br>

## 4.5 TodoForm.jsx

```jsx
import axios from "axios";
import { useAtom } from "jotai";
import styled from "styled-components";
import { addTodoAtom } from "store/todoAtom";
import { v4 as uuid } from "uuid";

const TodoForm = () => {
  const [, addTodo] = useAtom(addTodoAtom);

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
      addTodo(res.data);

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

## 4.6 TodoList.jsx

```jsx
import { useAtom } from "jotai";
import TodoItem from "./TodoItem";
import { todosAtom } from "store/todoAtom";
import styled from "styled-components";

const TodoList = () => {
  const [todos] = useAtom(todosAtom);

  const workingTodos = todos.filter((todo) => !todo.isDone);
  const doneTodos = todos.filter((todo) => todo.isDone);

  return (
    <>
      <StH2>미완료</StH2>
      {workingTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
      <StH2>할 일 완료</StH2>
      {doneTodos.map((todo) => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </>
  );
};

export default TodoList;

const StH2 = styled.h2`
  font-size: 2rem;
  padding: 3rem;
`;
```

<br>

## 4.7 TodoItem.jsx

```jsx
import axios from "axios";
import { useAtom } from "jotai";
import styled from "styled-components";
import { deleteTodoAtom, toggleTodoAtom, updateTodoAtom } from "store/todoAtom";
import { useState } from "react";

const TodoItem = ({ todo }) => {
  const [, deleteTodo] = useAtom(deleteTodoAtom);
  const [, toggleTodo] = useAtom(toggleTodoAtom);
  const [, updateTodo] = useAtom(updateTodoAtom);

  const [edit, setEdit] = useState(null);

  const handleDeleteButton = async (id) => {
    const deleteConfirm = window.confirm("정말 삭제하시겠습니까?");
    if (deleteConfirm) {
      try {
        await axios.delete(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`);
        deleteTodo(id);
        alert("삭제되었습니다.");
      } catch (error) {
        console.error("할 일을 삭제하는 중 오류가 발생했습니다:", error);
      }
    }
  };

  const handleEditButton = (todo) => {
    setEdit(todo);
  };

  const handleUpdateButton = () => {
    updateTodo(edit);
    alert("수정되었습니다.");
    setEdit(null);
  };

  const isDoneOnToggleButton = async (id) => {
    try {
      await axios.put(`${process.env.REACT_APP_SERVER_URL}/todos/${id}`, {
        ...todo,
        isDone: !todo.isDone,
      });
      toggleTodo(id);
    } catch (error) {
      console.error(
        "할 일의 완료 상태를 토글하는 중 오류가 발생했습니다:",
        error
      );
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

## 4.8 TodoSort.jsx

```jsx
import { useAtom } from "jotai";
import { sortTodosAtom } from "store/todoAtom";

const TodoSort = () => {
  const [, sortTodos] = useAtom(sortTodosAtom);

  const onChangeSortOrder = (e) => {
    const nextSortOrder = e.target.value;
    sortTodos(nextSortOrder);
  };

  return (
    <div>
      <select onChange={onChangeSortOrder}>
        <option value="asc">오름차순</option>
        <option value="desc">내림차순</option>
      </select>
    </div>
  );
};

export default TodoSort;
```

<br>
