---
title: "[TS] #6 TS로 TodoList 만들기/작성중(props-drillings)"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Todo 타입 정의

> `Todo`, `DoneTodo`, `InProgressTodo` 인터페이스로 기본 투두 항목의 구조와 상태에 따른 변형을 정의한다.

각 컴포넌트에서 사용할 타입을 미리 설정하여 타입 안정성을 보장.한다.

```tsx
// src/types/todo.type.ts

export interface Todo {
  id: string;
  title: string;
  content: string;
  isDone: boolean;
  deadline: string;
}

export interface DoneTodo extends Todo {
  isDone: true;
}

export interface InProgressTodo extends Todo {
  isDone: false;
}
```

<br><br>

# 2. useTodos 훅

> `useTodos`는 투두 리스트 상태 관리를 담당하는 커스텀 훅이다.

`addTodo`, `deleteTodo`, `toggleTodoDone` 함수와, 완료 상태에 따라 필터링된 `inProgressTodos`, `doneTodos` 리스트를 반환한다.

```tsx
// src/hooks/useTodos.tsx

import { useState } from "react";
import { DoneTodo, InProgressTodo, Todo } from "../types/todo.type";

export const useTodos = () => {
  const [todos, setTodos] = useState<Todo[]>([]);

  const inProgressTodos = todos.filter(
    (todo) => !todo.isDone
  ) as InProgressTodo[];

  const doneTodos = todos.filter((todo) => todo.isDone) as DoneTodo[];

  const addTodo = (todo: Todo) => {
    setTodos((prevTodos) => [todo, ...prevTodos]);
  };

  const deleteTodo = (id: string) => {
    setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
  };

  const toggleTodoDone = (id: string) => {
    setTodos((prevTodos) =>
      prevTodos.map((todo) =>
        todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
      )
    );
  };

  return {
    todos,
    addTodo,
    deleteTodo,
    toggleTodoDone,
    inProgressTodos,
    doneTodos,
  };
};
```

<br><br>

# 3. App 컴포넌트

> ` App` 컴포넌트는 전체 애플리케이션의 최상위 컴포넌트로, `useTodos` 훅을 사용하여 투두 데이터를 관리한다.

`addTodo`, `deleteTodo`, `toggleTodoDone` 같은 함수는 props drilling을 통해 하위 컴포넌트로 전달된다.

```tsx
// src/App.tsx
import TodoInput from "./components/todo/TodoForm";
import TodoList from "./components/todo/TodoList";
import { useTodos } from "./hooks/useTodos";

const App: React.FC = () => {
  const {
    addTodo,
    todos,
    deleteTodo,
    toggleTodoDone,
    inProgressTodos,
    doneTodos,
  } = useTodos();

  return (
    <>
      <TodoInput addTodo={addTodo} />
      <TodoList
        todoTitle="In Progress"
        todos={inProgressTodos}
        deleteTodo={deleteTodo}
        toggleTodoDone={toggleTodoDone}
      />
      <TodoList
        todoTitle="Done"
        todos={doneTodos}
        deleteTodo={deleteTodo}
        toggleTodoDone={toggleTodoDone}
      />
    </>
  );
};

export default App;
```

<br><br>

# 4. TodoForm 컴포넌트

> `TodoForm`은 새로운 투두 항목을 입력받아 `addTodo` 함수를 호출해 리스트에 추가한다.

FormData 객체를 사용하여 폼 데이터를 간편하게 가져온다.

```tsx
// src/components/todo/TodoForm.tsx

import { Todo } from "../../types/todo.type";

interface TodoFormProps {
  addTodo: (todo: Todo) => void;
}

const TodoForm = ({ addTodo }: TodoFormProps) => {
  const handleOnSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    // 요즘은 const title = e.target.title.value 이렇게 안함
    // name(title, content)은 html 코드기 때문에 js로 가져오기 힘들기 때문
    // 따라서 formData를 사용하여 form 태그 안에 있는 데이터를 가져오기
    const formData = new FormData(e.currentTarget);

    // title은 get으로 가져올 수 있음
    const title = formData.get("title") as string; // 타입 어쏘션(어쏘션: 단언하다)? 을 사용함으로써 type이 file이 아닌 text
    const content = formData.get("content") as string;

    const nextTodo: Todo = {
      id: crypto.randomUUID(),
      title,
      content,
      isDone: false,
      deadline: new Date().toLocaleDateString(),
    };

    addTodo(nextTodo);
  };

  return (
    <section>
      <form onSubmit={handleOnSubmit}>
        <input type="text" name="title" />
        <input type="text" name="content" />

        <button type="submit">제출</button>
      </form>
    </section>
  );
};

export default TodoForm;
```

<br><br>

# 5. TodoList 컴포넌트

> `TodoList`는 전달받은 `todos` 배열을 받아, 각 `TodoItem`에 필요한 함수들을 전달한다.

`props drilling`을 통해 부모 컴포넌트로부터 받은 `deleteTodo`, `toggleTodoDone`를 사용해 각 투두 항목의 상태를 변경한다.

```tsx
// src/components/todo/TodoList.tsx

import { Todo } from "../../types/todo.type";
import TodoItem from "./TodoItem";

interface TodoListProps {
  todoTitle: string;
  todos: Todo[];
  deleteTodo: (id: string) => void;
  toggleTodoDone: (id: string) => void;
}

const TodoList = ({ todos, deleteTodo, toggleTodoDone }: TodoListProps) => {
  return (
    <section>
      <h2>todo title</h2>

      <div>
        <ul>
          {todos.map((todo) => (
            <TodoItem
              key={todo.id}
              todo={todo}
              deleteTodo={deleteTodo}
              toggleTodoDone={toggleTodoDone}
            />
          ))}
        </ul>
      </div>
    </section>
  );
};

export default TodoList;
```

<br><br>

# 6. TodoItem 컴포넌트

> `TodoItem`은 각 투두 항목을 렌더링하며, 완료 상태를 토글하거나 항목을 삭제할 수 있는 버튼을 제공한다.

```tsx
// src/components/todo/TodoItem.tsx

import { Todo } from "../../types/todo.type";
import { StTodoCardItem } from "../../style/TodoStyle";

interface TodoItemProps {
  todo: Todo;
  deleteTodo: (id: string) => void;
  toggleTodoDone: (id: string) => void;
}

const TodoItem = ({ todo, deleteTodo, toggleTodoDone }: TodoItemProps) => {
  const { title, content, deadline, isDone, id } = todo;

  // 삭제
  const handleDelete = () => deleteTodo(id);

  // 토글
  const handleToggleDone = () => toggleTodoDone(id);

  return (
    <StTodoCardItem $isDone={isDone}>
      <article>
        <h3>{title}</h3>
        <p>{content}</p>
        <time>{deadline}</time>
        <div>
          <button onClick={handleToggleDone}>{isDone ? "취소" : "완료"}</button>
          <button onClick={handleDelete}>삭제</button>
        </div>
      </article>
    </StTodoCardItem>
  );
};

export default TodoItem;
```

<br><br>

# 7. styled-components 사용

> 투두 항목이 완료되었을 때 스타일을 변경한다.(텍스트에 line-through 적용)

```tsx
// src/style/TodoStyle.tsx

// styled-components도 컴포넌트이기 때문에 type을 선언해줘야한다.

import styled from "styled-components";

export const StTodoHeader = styled.header`
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  border-bottom: 1px solid #000;
  margin: 1rem;
`;

export interface StTodoCardItemProps {
  $isDone: boolean;
}

export const StTodoCardItem = styled.li<StTodoCardItemProps>`
  padding: 1rem;
  border: 1px solid #000;
  text-decoration: ${({ $isDone }) => ($isDone ? "line-through" : "none")};
`;
```

<br><br>

# 1. 비동기 함수에서 TS 사용하기

## 1.1 json 서버 열기

먼저 json 서버를 설치해주자

```shell
yarn add json-server
```

<br>

## 1.2 db.json 파일 생성

그 후, db.json 파일을 만들어 아래와 같은 내용을 넣어주자

```json
{
  "todos": [
    { "id": 1, "title": "오늘 날씨는", "content": "너무 더워요" },
    { "id": 2, "title": "타입스크립트는", "content": "재밌어요?" }
  ]
}
```

<br>

아래 명령을 통해 json-sever를 실행하자

```shell
yarn json-server --watch db.json --port 3001
```

<br>

## 1.2 비동기 함수와 TS

> 아래 비동기 함수를 보자

```ts
async function getPost() {
  const res = await fetch(`http://localhost:3001/people`);
  if (!res.ok) {
    throw new Error();
  }
  return res.json();
}
getPost().then((res) => console.log(res));
```

아래 명령어로 실행하기!

```
ts-node 파일명.ts
```

![](/assets/images/2024/2024-03-07-17-59-10.png)

res보면 any 타입인 것을 확인할 수 있다. any 타입은 모든 타입을 허용하기 때문에 (=치트키)TypeScript가 제공하는 강력한 정적 타입 검사 기능을 이용할 수 없게 된다.

<br>

> any 타입을 피하고 타입을 명시적으로 정의하여 타입 안정성을 보장하도록 해보자.

함수를 선언하는 부분에 `Promise<Person[]>` 작성하기

```ts
type Person = { id: number; age: number; height: number };

// getperson이 반환하는 건 person의 array
// 비동기 함수는 promise를 반환하기 때문에 promise 타입을 사용해야함
async function getperson(): Promise<Person[]> {
  const res = await fetch("http://localhost:3001/people");
  if (!res.ok) {
    throw new Error();
  }
  return res.json();
}

getperson().then((res) => console.log(res[0].age));
```

<br>

이 외에 return 부분에 type을 작성해줘도 함수의 type이 return부의 type으로 자동으로 추론되기 때문에 아래와 같이 작성해도 된다! (근데 Promise가 더 간단한 듯..)

```ts
type Person = { id: number; age: number; height: number };

async function getperson() {
  const res = await fetch("http://localhost:3001/people");
  if (!res.ok) {
    throw new Error();
  }
  return res.json() as any as Person[];
}

getperson().then((res) => console.log(res[0].age));
```

<br><br>

# 참조

- [코딩에 진심인 사람을 위해 준비한 리액트 타입스크립트 | 실제 회사에서 쓰는 레벨 ver](https://www.youtube.com/watch?v=V9XLst8UEtk&t=102s)

<br>
