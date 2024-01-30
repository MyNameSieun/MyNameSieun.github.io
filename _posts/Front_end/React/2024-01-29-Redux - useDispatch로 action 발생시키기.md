---
title: "[React] Redux - useDispatch로 action 발생시키기"
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

# 1. Redux의 데이터 흐름

![리덕스 흐름](<../../../assets/images/2024/리덕스 흐름.gif>)

1. Store 안에는 State도 있고 Reducer도 있다.
2. State는 상태이며, Reducer는 Swich문을 통해 action의 type에 따라 상태를 제어한다.<br><Br>
   ```js
   // 리듀서
   const counter = (state = initialState, action) => {
     switch (action.type) {
       default:
         return state;
     }
   };
   ```
3. UI(컴포넌트)에서 이벤트가 발생해서 store(중앙 데이터 관리소)에 있는 state를 바꿔야하는 상황이면(ex. onClick)<br> Dispatch가 스토어에게 <span style="color:indianred">액션 객체(type, payload)</span>를 전달한다. (Dispatch는 액션 객체를 가지고 스토어에게 던져주는 느낌⭐)
4. 스토어의 Reducer은 액션 객체에 있는 type에 따라 state를 변경한다.

<br><br>

# 2. state 수정 기능 만들기

counter.js 모듈의 state 수정 기능을 만들어보자. (+ 1 기능 구현)

> 어떻게 counter.js 모듈에 있는 state의 값을 변경할 수 있을까?

`useState()`를 사용해서 `number`에 +1을 할 때는 setNumber을 이용해서 +1을 해주었다.

```jsx
// 예시 코드

// local state
const [number, setNumber] = useState(0);

// click handler
const onClickHandler = () => {
  setNumber(number + 1);
};
```

<br>

하지만, 리덕스를 사용했을 때는 액션 객체를 만들어 스토어에게 전달하는 과정을 거쳐야한다. 리덕스에서 값의 수정은 스토어의 리듀서에서 일어나기 때문이다.

먼저 App.js에 아래와 같은 코드를 작성해주자.

```js
import logo from "./logo.svg";
import "./App.css";
import { useSelector } from "react-redux";

function App() {
  const date = useSelector((state) => state.counter); // 0

  return (
    <>
      <div>현재 카운트: {date.number}</div>
      <button
        onClick={() => {
          // +1을 해주는 로직을 써주면 된다.
        }}
      >
        +
      </button>
    </>
  );
}

export default App;
```

그 후, counter.js 모듈에 있는 number에 +1을 하고 싶으면 아래와 같이 하면 된다.

1. 스토어에있는 리듀서에게 보낼 number를 +1 하라는 액션 객체(명령)을 만든다.
2. 명령을 보낸다.
3. 리듀서에서 명령을 받아 number +1을 한다.

<br>

## 2.1 리듀서에게 보낼 액션 객체 만들기

스토어에있는 리듀서에게 number에 +1을 하라고 액션 객체(명령)을 보내야 한다.

액션 객체는 반드시 type이라는 `key`를 가져야 한다. 이 액션 객체를 리듀서에게 보냈을 때 리듀서는 객체 안에서 type이라는 key를 보기 때문이다.

```jsx
// src/redux/modules/counter.js

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  switch (action.type) {
    case "PLUS_ONE":
      return {
        // state는 객체이므로 객체를 리턴해야한다.
        number: state.number + 1,
      };
    default:
      return state;
  }
};

export default counter;
```

앞으로 리덕스 모듈에 있는 state을 변경하기 위해서는 그에 해당하는 액션 객체(ex. PLUS_ONE)를 모두 만들어줘야 한다.

<br>

## 2.2 액션 객체(명령) 보내기

액션 객체를 만들었으니, 리듀서에게 보내야 한다.

액션객체를 보내기 리듀서로 보내기위해서 `useDispatch`라는 훅을 사용해야한다.

react-redux에서 import 해서 사용할 수 있으며, 만든 액션 객체를 리듀서로 보내주는 역할을 하는 훅이다.

⚠️ 이렇게 생성한 dispatch는 함수이기 때문에 dispatch를 사용할 때 `()` 를 붙여서 함수를 실행해야한다!

인자로 액션 객체를 넣어주면 된다.

```jsx
// src/App.js

import React from "react";
import { useDispatch } from "react-redux"; // import.

const App = () => {
  const dispatch = useDispatch(); // dispatch 생성
  return (
    <div>
      <button>+ 1</button> {/* 버튼을 하나 추가해주세요. */}
    </div>
  );
};

export default App;
```

<br>

그리고 dispatch를 사용할 때 ( ) 안에 액션객체를 넣어주면 된다.

만약 어떤 버튼을 클릭했을 때 리듀서로 액션객체를 보내고 싶다면 아래와 같이 코드를 작성하면 된다.

```jsx
// src/App.js
import logo from "./logo.svg";
import "./App.css";
import { useDispatch, useSelector } from "react-redux"; // import

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
            type: "PLUS_ONE",
          });
        }}
      >
        +
      </button>
    </>
  );
}

export default App;
```

이렇게 디스패치를 이용해서 액션객체를 리듀서로 보낼 수 있다.

<br>

## 2.3 액션 객체 받기

액션객체를 리듀서로 보냈으니, 리듀서에서 액션객체가 잘 왔는지 App.js에서 보낸 액션객체를 받아보자.

```jsx
import logo from "./logo.svg";
import "./App.css";
import { useDispatch, useSelector } from "react-redux";

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
            type: "PLUS_ONE",
          });
        }}
      >
        +
      </button>
    </>
  );
}

export default App;
```

<br>

> 마이너스도 구현해보자! 마이너스 로직만 추가해주면 된다.

```js
// App.jsx
import logo from "./logo.svg";
import "./App.css";
import { useDispatch, useSelector } from "react-redux";

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
            type: "PLUS_ONE",
          });
        }}
      >
        +
      </button> <button
        onClick={() => {
          // -1을 해주는 로직을 써주면 된다.
          dispatch({
            type: "MINUS_ONE",
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

```js
// src/redux/modules/counter.js

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서
const counter = (state = initialState, action) => {
  switch (action.type) {
    case "PLUS_ONE":
      return {
        number: state.number + 1,
      };
    case "MINUS_ONE":
      return {
        number: state.number - 1,
      };
    default:
      return state;
  }
};

export default counter;
```

![](/assets/images/2024/2024-01-29-17-47-42.png)

<br>
