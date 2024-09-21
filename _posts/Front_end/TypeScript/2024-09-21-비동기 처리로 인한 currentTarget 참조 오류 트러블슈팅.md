---
title: "[TS] 비동기 처리로 인한 currentTarget 참조 오류 트러블슈팅💫"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 발생한 문제 🤦‍♀️

> handleOnSubmit 함수에서 `TypeError: Cannot read properties of null (reading 'reset')` 이라는 오류가 발생하였다.

![](/assets/images/2024/2024-09-21-20-12-47.png)

```tsx
const handleOnSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();

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

  await createTodo(nextTodo);
  alert("추가 완료!");
  e.currentTarget.reset(); // 오류 발생

  const response = await fetchTodo();
  setTodos(response && response.data);
};
```

<br>

# 문제 원인 🤷‍♀️

> 이 오류는 `e.currentTarget`이 `null`이 되어 `reset()` 메소드를 호출할 수 없기 때문에 발생하는 오류이다.

- `handleOnSubmit` 함수가 비동기로 `createTodo`를 호출한 후에 상태가 변경되었으므로 `e.currentTarget`은 더 이상 유효하지 않게 되어 `reset()` 메소드를 호출할 수 없었던 것이였다.
- 즉, `currentTarget`은 참조할 수 없는 상태가 되어버린 것이다.

<br>

# 해결 방안 💁‍♀️

> 위와 같은 이유 때문에, `currentTarget`을 직접 사용하기보다는 해당 값을 변수에 저장하여 비동기 작업이 완료된 후에도 참조할 수 있도록 해야한다.

따라서, 초기 `form` 요소를 변수에 저장하여 `formElement`가 해당 시점의 `currentTarget`을 참조하도록 로직을 변경하였다.

```tsx
const handleOnSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();

  const form = e.currentTarget; // form 요소를 변수에 저장
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

  await createTodo(nextTodo);
  alert("추가 완료!");
  form.reset(); // 저장된 form 변수를 사용

  const response = await fetchTodo();
  setTodos(response && response.data);
};
```

<br>
