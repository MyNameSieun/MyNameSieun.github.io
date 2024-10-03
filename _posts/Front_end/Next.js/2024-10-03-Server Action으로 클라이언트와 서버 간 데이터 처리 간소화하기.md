---
title: "[Next.js] Server Action으로 클라이언트와 서버 간 데이터 처리 간소화하기(조금 더 공부하기)"
categories: [Next.js]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 서버 액션 개요

## 1.1 서버 액션의 개념

> 서버 액션은 Next.js 13.4에서 도입된 기능으로, 클라이언트에서 발생한 이벤트나 요청을 **서버에서 처리**하는 비동기 함수이다.

- 서버에서 데이터베이스 작업, 파일 시스템 접근 등 복잡한 서버 로직을 클라이언트에서 쉽게 실행할 수 있다.
- 클라이언트는 단순히 서버 액션을 호출하고, 서버가 반환한 데이터를 받는 형태로 동작한다.

<br>

## 1.2 서버 액션의 작동 원리

1. 클라이언트에서 서버 액션 호출: 클라이언트 컴포넌트에서 직접 서버 액션을 호출한다.
2. 서버에서 액션 실행: 서버는 액션을 처리하여 비즈니스 로직을 실행한다.
3. 서버에서 결과 반환 및 UI 업데이트: 서버는 처리 결과를 클라이언트로 반환하고, UI가 업데이트된다.ㄴ

<br>

## 1.3 서버 액션의 장점

> 서버 사이드 로직의 분리

- 서버 액션은 비즈니스 로직을 서버에서 처리하고, 클라이언트는 단순히 데이터를 요청하는 방식이다.
- 이를 통해 서버와 클라이언트의 역할을 명확히 구분할 수 있어, 코드가 더 깔끔하고 유지보수가 용이해진다.

<br>

> 성능 최적화

- 서버에서 데이터를 처리하고 응답을 보내는 방식은 네트워크 요청 횟수를 줄이고 데이터 처리 부담을 서버에 맡긴다
- 이는 초기 로딩 시간을 단축시키고 클라이언트 성능을 개선하는 데 도움이 된다.

<br>

> 보안 강화

- 서버 액션은 서버에서 민감한 로직이나 데이터를 처리하므로, 클라이언트에서 직접적으로 중요한 정보를 다루지 않게 된다.
- 서버 사이드에서만 데이터가 처리되어 보안상 더 안전한 환경을 제공한다.

<br>

## 1.4 서버 액션의 활용

> 폼 처리

- 클라이언트에서 폼을 제출하면 서버 액션을 통해 데이터베이스에 저장하거나 파일을 업로드
- ex: 회원 가입, 주문 처리.

<br>

> 클라이언트에서 데이터를 가져오는 대신 서버에서 데이터 처리 후 전달

- 데이터를 서버에서 직접 처리하고 클라이언트에 전달
- ex: 서버 사이드 렌더링(SSR), 초기 페이지 로딩

<br>

> 보안이 중요한 작업 처리

- 클라이언트에서 민감한 정보 처리 없이 서버에서만 처리하고 결과만 클라이언트에 전달. (클라이언트는 단순히 결과를 받는다.)
- ex: 로그인, 결제 처리

<br>

## 1.4 데이터 패칭 vs 서버 액션

- 데이터 패칭
  - 클라이언트가 서버에서 데이터만 요청하여 가져온다.
  - 주로 `GET` 요청을 사용하여 데이터를 읽기 전용으로 가져오는 데 사용된다.
- 서버 액션
  - 서버에서 로직 처리 후 결과를 클라이언트에 전달한다.
  - `POST`, `PUT`, `DELETE` 등을 사용하여 데이터 변경 작업을 처리한다.

💡 데이터 패칭은 데이터를 읽기 위해 클라이언트가 서버에서 요청하는 방식이고, 서버 액션은 서버에서 로직을 처리하고 데이터를 변경하는 작업을 클라이언트에 전달하는 방식이다.<br>
⚠️ `GET`요청은 서버 액션에서 사용할 수 없다! `GET` 요청은 단순한 데이터 조회를 위해 주로 클라이언트에서 사용되고, Next.js 서버 액션에서는 `POST`, `PUT`, `DELETE`와 같은 데이터 변경 작업만 처리할 수 있기 때문이다.

<br><br>

# 2. 서버 액션 사용하기

서버 액션을 사용하려면, 먼저 서버 액션을 정의하고, 그 후 클라이언트에서 호출하여 데이터를 처리하면 된다.

## 2.1 서버 액션 정의하기

> `"use server"`라는 지시어를 함수 최상단에 추가하면 해당 함수가 서버에서 실행되도록 할 수 있다.

```tsx
// app/action/todo-actions.ts
"use server";

import { Todo } from "../types/todo-types";

const baseURL = process.env.NEXT_PUBLIC_SERVER_URL;

export const addTodo = async (todo: Todo) => {
  const response = await fetch(`${baseURL}/todos`, {
    method: "post",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(todo),
  });
  const data = await response.json();

  return data;
};
```

- 위 코드에서 `addTodo`라는 함수가 서버에서 실행되며, 할 일을 `POST` 방식으로 서버에 전송하고 결과를 반환한다.
- 클라이언트에서 이 서버 액션을 호출하여 사용자가 리스트를 가져올 수 있다.

<br>

## 2.2 클라이언트에서 서버 액션 호출하기

> 클라이언트는 서버 액션을 호출하기만 하고, 그에 대한 응답을 받아 처리한다.

mutation을 사용하여 서버 액션(addTodo)을 클라이언트에서 호출해주었다.

```tsx
export const useAddTodoMutation = () => {
  const queryClient = useQueryClient();

  // Add Todo Mutation
  const { mutate: addTodoMutate } = useMutation({
    mutationFn: addTodo,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: [QUERY_KEYS.TODOS] });
      alert("추가 완료!");
    },
    onError: (error) => {
      console.error("추가 실패:", error);
      alert("추가 실패. 다시 시도해 주세요.");
    },
  });

  return { addTodoMutate };
};
```

<br>

> 이제 `addTodoMutate`를 통해 서버 액션을 호출할 수 있다.

```tsx
"use client";

import { useAddTodoMutation } from "@/app/hooks/query/useTodosMutation";
import { useId } from "react";

const TodoForm = () => {
  // 추가
  const { addTodoMutate } = useAddTodoMutation();

  const handleOnSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    const form = e.currentTarget;
    const formData = new FormData(e.currentTarget);
    const title = formData.get("title") as string;
    const content = formData.get("content") as string;

    const nextTodo = {
      id: crypto.randomUUID(),
      title,
      content,
      isDone: false,
      deadline: new Date().toLocaleDateString(),
    };

    addTodoMutate(nextTodo);

    form.reset();
  };

  const titleId = useId();
  const contentId = useId();

  return (
    <div>
      <form onSubmit={handleOnSubmit}>
        <label htmlFor={titleId}>Title</label>
        <input type="text" id={titleId} name="title" required />
        <label htmlFor={contentId}>content</label>
        <input type="text" id={contentId} name="content" required />
        <button type="submit">추가하기</button>
      </form>
    </div>
  );
};

export default TodoForm;
```

> 서버 액션에서 삭제 작업을 실행한 후, 다음과 같은 데이터가 반환되었다!

- 그리고 `DELETE`, `PATCH`와 같은 상태 코드를 반환하지 않았다. (`GET`, `POST` 만 반환)

![](/assets/images/2024/2024-10-03-20-44-59.png)

<br>

- ❓ 서버 액션에서 Request Method에 `GET`, `POST`만 반환되고 `DELETE`, `PATCH`의 는 반환되지 않는 이유를 찾아봤더니,
  - 주로 서버 구성, 클라이언트의 요청 방식, 또는 프레임워크의 제한에 의해 발생한다고 한다.
  - (GPT한테 물어본 것이므로 확실하지 않다.. 다른 분들한테 물어봐야겠다. 진짜 왜 그런거지?)

<br>

> 반면, 서버 액션을 사용하지 않았을 경우 `GET`, `DELETE`, `PATCH`와 같은 상태 코드만 반환이 되었다.

삭제된 항목의 정보는 **응답(response)**에 포함되지 않았다.

![](/assets/images/2024/2024-10-03-22-14-35.png)![](/assets/images/2024/2024-10-03-22-14-46.png)

<br><br>

# 3. 클라이언트와 서버 로직을 같은 컴포넌트 안에서 처리하기

서버 로직을 클라이언트 컴포넌트 안에서 함께 작성할 수 있다.

## 3.1 동작 원리

> `Server Actions`을 사용하면 클라이언트에서 action 속성으로 서버 코드가 포함된 함수를 호출할 수 있다.

- 이때 서버 코드는 자동으로 서버에서 실행된다.
- 즉, 클라이언트 컴포넌트에서 서버 코드를 직접 작성하고, 이를 서버에서만 실행되도록 할 수 있다는 것이다.

<br>

> 기존 방식 (클라이언트와 서버 분리)

- 기존 방식에서는 클라이언트 컴포넌트에서 폼을 제출하면, 서버로 API 요청을 보내서 데이터를 처리한다.
- 예를 들어, 클라이언트에서 서버에 `POST` 요청을 보내서 데이터를 저장한다고 가정해보자.

1. 클라이언트 컴포넌트에서 서버 API로 요청
2. 서버에서 요청을 받아 DB에 저장

💡 하지만, 서버 액션을 사용하면 별도로 API를 작성할 필요 없이, 클라이언트에서 서버 로직을 바로 처리할 수 있다.

<br>

> 서버 액션을 사용하여 폼 제출을 통해 서버에서 DB에 데이터를 저장하는 예시

```tsx
// app/write2/page.js
import { connectDB } from "@/util/database"; // DB 연결 유틸 함수

export default async function Write2() {
  // 서버 액션 함수 (서버 코드)
  async function handleSubmit(formData) {
    "use server"; // 서버 코드임을 표시
    const db = (await connectDB).db("forum");
    await db
      .collection("post_test")
      .insertOne({ title: formData.get("post1") });
  }

  // 폼 구성
  return (
    <form action={handleSubmit}>
      <input type="text" name="post1" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

- 클라이언트 컴포넌트: UI와 사용자의 입력을 받는 역할을 한다.
- 서버 코드: 폼 제출 시 데이터를 처리하고 DB에 저장하는 작업을 서버에서만 실행한다.

💡 이렇게 서버와 클라이언트 로직이 같은 파일 안에서 처리할 수 있다. 즉, 별도로 API 파일을 작성할 필요 없이, 서버와 클라이언트 로직을 하나의 컴포넌트에서 동시에 관리할 수 있는 것이다.

<br><br>

# 참조

- [[nextjs] Server Action](https://velog.io/@jjunyjjuny/nextjs-Server-Action)
- [Next.js의 Server actions 기능](https://codingapple.com/unit/nextjs-server-actions/)

<br>
