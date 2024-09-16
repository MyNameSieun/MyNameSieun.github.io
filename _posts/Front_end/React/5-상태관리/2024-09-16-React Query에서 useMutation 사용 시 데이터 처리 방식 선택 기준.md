---
title: "[React] React Query에서 useMutation 사용 시 데이터 처리 방식 선택 기준"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 개요

> React Query의 `useMutation` 훅을 사용하여 데이터를 서버로 전송할 때, `mutationFn` 내에서 직접 데이터를 정의하거나 `mutate` 메서드를 통해 동적으로 데이터를 전달할 수 있다.

- 리액트 쿼리를 공부하던 중, 어떤 방식을 채택해야 할지 고민이 되어 이 포스팅을 작성하게 되었다!
- 두 방식의 차이점과 각각의 사용 시점에 대해 알아보자.

<br><br>

# 2. mutationFn 내에서 직접 데이터 정의하기

> `mutationFn` 내부에서 컴포넌트의 상태나 정적인 데이터를 직접 참조하여 mutation을 실행하는 방법이다.

데이터를 한 번만 사용할 경우나, 컴포넌트 내에서 상태 값이 변경되지 않는 경우 즉, 정적인 데이터를 처리해야 할 경우 사용하기 적합하다.

```jsx
const editPostMutation = useMutation({
  mutationFn: () => updatePost(id, { title, content }), // 상태를 직접 참조
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ["posts"] });
    setIsEditing(false);
    alert("게시글이 수정되었습니다.");
  },
  onError: (error) => {
    alert(error.message);
  },
});
```

<br><br>

# 3. mutate 메서드를 통해 동적으로 데이터 전달하기

> `mutate` 메서드를 호출할 때마다 인자를 동적으로 전달하여 mutation을 실행하는 방식이다.

주로 사용자 입력에 따라 다양한 데이터를 전송해야 할 때 즉, 동적인 데이터를 처리해야 할 경우 사용하기 적합하다.

```jsx
const editPostMutation = useMutation({
  mutationFn: ({ id, title, content }) => updatePost(id, { title, content }), // 동적 데이터 사용
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ["posts"] });
    setIsEditing(false);
    alert("게시글이 수정되었습니다.");
  },
  onError: (error) => {
    alert(error.message);
  },
});

const handleSaveButton = () => {
  editPostMutation.mutate({ id, title, content }); // 동적으로 데이터 전달
};
```

<br><br>

# 4. 정리

- 데이터가 정적이거나 컴포넌트 내부에서 쉽게 참조할 수 있을 때는 `mutationFn` 내에서 직접 데이터를 정의하는 것이 더 간단하다.
- 반면, 동적인 데이터를 처리하거나 외부에서 데이터를 받아와야 할 때는 `mutate` 메서드로 데이터를 전달하는 방식이 유리하다.

<br>
