---
title: "[React] React Hooks - useReducer"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. useReducer

## 1.1 useReducer 개요

> useReducer는 useState 보다 복잡한 상태 관리가 필요한 상황에서 유용하다.

useReducer는 React의 훅 중 하나로, 상태 관리와 관련된 로직을 보다 체계적으로 다룰 수 있도록 도와준다. 주로 상태가 복잡하거나 여러 액션을 통해 상태를 업데이트해야 하는 경우에 유용하다.

<br>

## 1.2 기본 개념

> useReducer는 리듀서 함수와 초기 상태를 인수로 받아, 현재 상태와 디스패치 함수를 반환한다.

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

- reducer: 상태와 액션을 받아 새로운 상태를 반환하는 함수이다.
- initialState: 초기 상태이다.
- state: 현재 상태를 나타내는 변수이다.
- dispatch: 액션 객체를 전달하여 상태를 업데이트하는 함수이다. 이 함수는 액션을 받아 리듀서 함수를 호출하고, 새로운 상태를 반환한다.

<br>

## 1.3 사용 방법

```jsx
import { useReducer } from "react";

// 초기 상태 정의
const initialState = { count: 0 };

// 액션 타입을 상수로 정의
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 리듀서 함수 정의
function reducerCounter(state, action) {
  // action.type에 따라 action.payload 만큼을 state에 반영해준다.
  switch (action.type) {
    case INCREMENT:
      return { count: state.count + 1 };
    case DECREMENT:
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  // useReducer 훅 호출
  const [state, dispatch] = useReducer(reducerCounter, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-</button>
    </div>
  );
}
export default Counter;
```

<br>

💡 useReducer + useContext = redux!

<br>
