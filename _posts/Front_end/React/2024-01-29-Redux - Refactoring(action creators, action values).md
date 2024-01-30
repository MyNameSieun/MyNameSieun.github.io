---
title: "[React] Redux - Refactoring(action creators, action values)"
categories: [React]
tag: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Action Creator 개요

## 1.1 Action Creator의 필요성

만약 액션객체의 value를 변경할 일이 생긴다면 어떨까?

`PLUS_ONE`, `MINUS_ONE` 이라는 value 대신 `counter/PLUS_ONE`, `counter/MINUS_ONE` 이라는 value로 바꾸길 원한다면 모든 컴포넌트에서 찾아 하나하나씩 바꿔줘야 할 것이다.

따라서 액션이라는 type을 문자열의 형태로 dispatch 또는 reduce에게 직접 하드코딩으로 넣는 것이 아니라, 상수 형태로 관리하는 것이다.

```jsx
// src/redux/modules/counter.js

// action value
export const PLUS_ONE = "counter/PLUS_ONE"; // value는 상수로 생성
export const MINUS_ONE = "counter/MINUS_ONE";

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
export const counter = (state = initialState, action) => {
  // plusOne()는 밖으로 나가서 사용될 예정이기 때문에 export 해주기
  switch (action.type) {
    case PLUS_ONE: // type에는 위에서 만든 상수로 사용
      return {
        number: state.number + 1,
      };
    case MINUS_ONE:
      return {
        number: state.number - 1,
      };
    default:
      return state;
  }
};

export default counter;
```

액션의 value는 상수로 따로 만들어주고, 그리고 그것을 이용해서 액션객체를 반환하는 함수를 작성한 후, 리듀서와 컴포넌트에서는 아래와 같이 작성한다.

```jsx
// App.jsx
import logo from "./logo.svg";
import "./App.css";
import { useDispatch, useSelector } from "react-redux";
import { PLUS_ONE } from "./redux/modules/counter";
import { MINUS_ONE } from "./redux/modules/counter";

function App() {
  const date = useSelector((state) => state.counter);

  // dispatch를 가져와보자
  const dispatch = useDispatch();

  return (
    <>
      <div>현재 카운트: {date.number}</div>
      <button
        onClick={() => {
          // +1을 해주는 로직을 써주면 된다.
          dispatch({
            type: PLUS_ONE,
          });
        }}
      >
        +
      </button> <button
        onClick={() => {
          // -1을 해주는 로직을 써주면 된다.
          dispatch({
            type: MINUS_ONE,
          });
        }}
      >
        -
      </button>
    </>
  );
}

export default App;
```

<br>

이제 이름을 바꿀 일이 있으면 아래 부분만 바꿔주면 되는 것이다!

```jsx
// src/redux/modules/counter.js
export const PLUS_ONE = "counter/PLUS_ONE";
export const MINUS_ONE = "counter/MINUS_ONE";
```

<br>

## 1.2 Action Creator 만들기

액션 객체를 만들어주는 함수를 따로 만들어 휴먼 에러를 방지하자!

액션을 만드는 생성자를 Action Creator라고 부른다.

```jsx
// App.jsx
...
import { plusOne } from "./redux/modules/counter";

  <button
        onClick={() => {
          dispatch(plusOne());
        }}
      >
        +
      </button>{" "}
      <button
        onClick={() => {
          dispatch(minusOne());
        }}
      >
        -
      </button>
...
```

```jsx
// src/redux/modules/counter.js
...
// action creator: action value를 return하는 함수
export const plusOne = () => {
  return {
    type: PLUS_ONE,
  };
};
export const minusOne = () => {
  return {
    type: MINUS_ONE,
  };
};
...
```

<br>
