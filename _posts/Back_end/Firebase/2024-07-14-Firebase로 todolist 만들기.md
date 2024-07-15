---
title: "[Firebase] Firebase로 todolist 만들기"
categories: [Firebase]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> [[여기↗️]](https://mynamesieun.github.io/firebase/Firebase-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0/) 링크를 참고하여 파이어베이스를 세팅하자!

<br>

# 1. 데이터 읽기

```jsx
useEffect(() => {
  const fetchTodos = async () => {
    const querySnapshot = await getDocs(collection(db, TODOS_COLLECTION));

    const nextTodos = querySnapshot.docs.map((doc) => ({
      ...doc.data(),
      id: doc.id,
    }));

    setTodos(nextTodos);
  };

  fetchTodos();
}, []);
```

<br>

# 2. 데이터 추가

```jsx
const onSubmitTodo = async (nextTodo) => {
  const doc = await addDoc(collection(db, TODOS_COLLECTION), nextTodo);

  const nextTodoWithServerId = {
    ...nextTodo,
    id: doc.id,
  };

  // optimistic update
  setTodos((prevTodos) => [nextTodoWithServerId, ...prevTodos]);
};
```

<br>

# 3. 데이터 삭제

```jsx
const onDeleteTodoItem = async (id) => {
  await deleteDoc(doc(db, TODOS_COLLECTION, id));

  setTodos((prevTodos) => prevTodos.filter((todo) => todo.id !== id));
};
```

<br>

# 4. 데이터 수정

```jsx
const onToggleTodoItem = async (id) => {
  // 서버의 상태를 가져와서 업데이트
  const todoRef = doc(db, TODOS_COLLECTION, id);
  const docSnap = await getDoc(todoRef);

  await updateDoc(todoRef, {
    isDone: !docSnap.data().isDone,
  });

  // 또는 클라이언트의 데이터를 가져와서 업데이트
  // const todo = todos.find((todoItem) => todoItem.id === id);
  // await updateDoc(doc(db, TODOS_COLLECTION, id), {
  //   isDone: !todo.isDone,
  // });

  setTodos((prevTodos) =>
    prevTodos.map((todoItem) => {
      if (todoItem.id === id) {
        return {
          ...todoItem,
          isDone: !todoItem.isDone,
        };
      }

      return todoItem;
    })
  );
};
```

<br>
