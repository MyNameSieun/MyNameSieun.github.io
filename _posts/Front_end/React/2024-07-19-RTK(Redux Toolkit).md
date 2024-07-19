---
title: "[React] RTK(Redux Toolkit)"
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

## 1.1 RTK란?

Redux Toolkit (RTK)은 Redux를 더 쉽게 사용하고 복잡성을 줄이기 위해 만들어진 라이브러리이다.

<br>

## 1.2 RTK 사용 이유

- 간결한 코드: RTK는 createSlice, configureStore 등의 유틸리티를 통해 보일러플레이트 코드를 줄여줘 개발자가 애플리케이션의 실제 로직에 더 집중할 수 있게 해준다.
- 미들웨어 및 DevTools 설정 자동화: configureStore는 Redux DevTools와 기본 미들웨어 설정을 자동으로 처리해준다.
- 비동기 작업 간소화: createAsyncThunk는 비동기 작업의 상태를 쉽게 관리할 수 있도록 도와준다.

<br>

## 1.3 설치

```
yarn add @reduxjs/toolkit
```

<br><br>

# 2. RTK 메서드

## 2.1 configureStore

- 목적: Redux 스토어의 설정과 생성을 간소화한다.
- 특징: `configureStore`는 미들웨어와 Redux DevTools를 자동으로 설정해준다.

{% raw %}

```jsx
import { configureStore } from "@reduxjs/toolkit";
import rootReducer from "./reducers";

// configureStore를 사용하여 Redux 스토어를 설정하고 생성한다.
// 이 함수는 Redux DevTools 및 Thunk 미들웨어를 자동으로 활성화한다.
const store = configureStore({
  // rootReducer를 통해 여러 리듀서를 하나로 합친다.
  reducer: rootReducer,
  // 추가 설정이 필요한 경우, 여기에 다른 옵션들을 지정할 수 있다.
});
```

{% endraw %}

<br>

## 2.2 createSlice

- 목적: 액션 타입, 액션 생성자, 리듀서 함수를 한 번에 생성한다.
- 특징: `createSlice`는 이름, 초기 상태, 리듀서 함수를 객체 형태로 받아, 액션 생성자와 액션 타입을 자동으로 생성한다. 또한 lmmer 라이브러리를 내장하고 있어, 상태 업데이트 로직을 불변성을 유지하면서 간결하게 작성할 수 있다.

{% raw %}

```jsx
import { createSlice } from "@reduxjs/toolkit";

// ceateSlice를 사용하여 todos에 대한 리듀서와 액션 생성자를 한 번에 정의한다.
const todoSlice = createSlice({
  name: "todos", // 슬라이스의 이름, 생성되는 액션 타입의 접두사로 사용
  initialState: [], // 상태의 초기값을 설정
  reducers: {
    // 리듀서 로직과 액션들을 정의
    // todo 항목을 추가하는 리듀서.
    // 액션의 payload를 현재 상태에 직접 push. Immer 덕분에 불변성을 신경쓰지 않아도 됨
    addTodo: (state, action) => {
      state.push(action.payload);
    },
    // todo 항목을 제거하는 리듀서
    // 액션의 payload로 전달된 id와 일치하지 않는 항목만 필터링하여 새 배열을 반환
    removeTodo: (state, action) => {
      return state.filter((todo) => todo.id !== action.payload);
      // const targetIndex = findIndex((todo) => todo.id === action.payload);
      // state.splice(targetIndex, 1);
    },
  },
});

// 생성된 액션 생성자 함수와 리듀서를 각각 내보냄
export const { addTodo, removeTodo } = todoSlice.actions;
export default todoSlice.reducer;
```

{% endraw %}

<br>

## 2.3 createAsyncThunk

- 목적: 비동기 로직을 처리하기 위한 Thunk 함수를 생성
- 특징: `createAsyncThunk`는 비동기 함수를 인자로 받고, 해당 비동기 함수의 실행 결과에 따라 "pending", "fulfilled", "rejected" 상태의 액션을 자동으로 디스패치한다. 이를 통해 비동기 로직을 쉽게 Redux 스토어와 연동할 수 있다.

{% raw %}

```jsx
import { createAsyncThunk } from "@reduxjs/toolkit";
import axios from "axios";

// createAsyncThunk를 사용하여 비동기적으로 todos를 불러오는 Thunk 액션 생성자를 정의한다.
// "todos/fetchTodos"는 생성될 액션 타입의 접두사로 사용된다.
export const fetchTodos = createAsyncThunk("todos/fetchTodos", async () => {
  // axios를 사용해 비동기적으로 데이터를 가져온다.
  const response = await axios.get("https://api.example.com/todos");
  // 응답 데이터를 Thunk의 반환값으로 지정한다. 이 값은 리듀서에서 처리된다.
  return response.data;
});
```

{% endraw %}

<br>

## 2.4 extraReducer

- 목적: `createAsyncThunk`로 정의한 함수를 사용하는 방법
- 특징: `createSlice` 내에서 비동기 액션의 결과(상태)를 다룰 수 있다.

{% raw %}

```jsx
import { createSlice } from "@reduxjs/toolkit";
import { fetchTodos } from "./fetchTodos"; // 비동기 액션 생성자를 임포트한다.

const todosSlice = createSlice({
  name: "todos",
  initialState: {
    items: [], // 실제 todo 항목들을 저장할 배열
    status: "idle", // "idle", "loading", "succeeded", "failed"
    error: null, // 요청 실패 시 에러 메시지 저장
  },
  reducers: {
    // 일반 동기 액션 핸들러들을 정의할 수 있다.
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state) => {
        // 비동기 요청이 시작될 때 상태를 업데이트한다.
        state.status = "loading";
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        // 요청이 성공적으로 완료됐을 때 상태를 업데이트한다.
        state.status = "succeeded";
        // 불러온 todos를 상태에 추가한다.
        state.items = action.payload;
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        // 요청이 실패했을 때 상태를 업데이트한다.
        state.status = "failed";
        state.error = action.error.message;
      });
  },
});

export default todosSlice.reducer;
```

{% endraw %}

<br>
