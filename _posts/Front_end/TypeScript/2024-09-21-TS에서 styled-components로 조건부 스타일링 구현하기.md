---
title: "[TS] TS에서 styled-components로 조건부 스타일링 구현하기"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# TS에서 styled-components로 조건부 스타일링 구현하기

- TypeScript와 styled-components를 함께 사용하면 props로 전달된 데이터를 기반으로 조건부 스타일링을 할 수 있다.
- styled-components도 컴포넌트이기 때문에, 해당 컴포넌트에 대한 type을 선언해줘야한다.

<br>

# 예시: 완료된 할 일 표시하기

> TodoStyle.tsx

- Todo 리스트에서 완료된 항목에 취소선을 그려서 표시하는 간단한 예시이다.
- 완료 여부는 `$isDone` 이라는 prop으로 전달하고, 이 prop에 따라 스타일이 다르게 적용된다.

{% raw %}

```tsx
// src/style/TodoStyle.tsx

import styled from "styled-components";

// Props의 타입 정의
export interface StTodoCardItemProps {
  $isDone: boolean; // 할 일이 완료되었는지 여부
}

// styled-components에서 li 태그에 스타일을 적용
export const StTodoCardItem = styled.li<StTodoCardItemProps>`
  padding: 1rem;
  border: 1px solid #000;
  text-decoration: ${({ $isDone }) =>
    $isDone ? "line-through" : "none"}; // 조건부 스타일링
`;
```

{% endraw %}

<br>

> TodoList.tsx

- 아래 코드를 실행하면 완료된 할 일 항목에는 취소선이 그려지고, 완료되지 않은 항목은 평범한 텍스트로 출력된다.
- 즉, `$isDone`이라는 boolean 값에 따라 동적으로 스타일을 변경한 것이다.

```tsx
// src/components/todo/TodoItem.tsx

import { StTodoCardItem } from "../../style/TodoStyle";
import { Todo } from "../../types/todo.type";

interface TodoItemProps {
  todo: Todo;
  deleteTodo: (id: string) => void;
  toggleTodoDone: (id: string) => void;
}

const TodoItem = ({ todo, deleteTodo, toggleTodoDone }: TodoItemProps) => {
  const { title, content, deadline, isDone, id } = todo;

  return (
    <StTodoCardItem $isDone={isDone}>
      <article>
        <p>제목: {title}</p>
        <p>내용: {content}</p>
        <p>등록일: {deadline}</p>
      </article>
    </StTodoCardItem>
  );
};

export default TodoItem;
```

![](/assets/images/2024/2024-09-21-15-09-01.png)

<br>
