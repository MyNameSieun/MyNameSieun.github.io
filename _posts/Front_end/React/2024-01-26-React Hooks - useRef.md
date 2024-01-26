---
title: "[React] React Hooks - useRef"
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

# 1. useRef 개요

## 1.1 useRef 개념

DOM 요소에 접근할 수 있도록 하는 React Hook이다.

JS에서 `getElementById`, `querySelector`를 이용하여 특정 DOM을 선택한 것 처럼 리액트에서도 useRef hook을 사용해 DOM을 선택할 수 있다.

<br>

## 1.2 useRef 사용 방법

```jsx
import "./App.css";
import { useRef } from "react";

function App() {
  const ref = useRef("초기값");
  console.log("ref", ref);

  return (
    <div>
      <p>useRef</p>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-26-15-33-26.png)

<br>

변경도 가능하다.

```jsx
import "./App.css";
import { useRef } from "react";

function App() {
  const ref = useRef("초기값");
  console.log("ref 1", ref);

  ref.current = "변경 값";
  console.log("ref 1", ref);

  return (
    <div>
      <p>useRef</p>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-26-15-34-21.png)

<br>

## 1.2 useRef의 용도

> ⭐⭐ 이렇게 설정된 ref 값은 컴포넌트가 계속해서 렌더링 되어도 <span style="color:indianred">unmount 전까지 값을 유지</span>한다.

- 이러한 특징 때문에 useRef는 다음 2가지 용도로 사용된다.

  1. state는 리렌더링이 꼭 필요한 값을 다룰 때 쓰면 된다.
  2. ref는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.
     <br><br>

- 저장공간
  - state와 비슷한 역할을 하지만, state는 변화가 일어나면 다시 렌더링이 일어난다. (내부 변수 초기화)
  - ref에 저장한 값은 렌더링을 일으키지 않는다. 즉, ref의 값 변화가 일어나도 렌더링으로 인해 내부 변수들이 초기화 되는 것을 막을 수 있다.
  - 컴포넌트가 100번 렌더링 → ref에 저장한 값은 유지
- DOM
  - 렌더링 되자마자 특정 input이 focusing 돼야 한다면 useRef를 사용하면 된다.

<br><br>

# 2. useRef의 특징

## 2.1 state와 ref의 차이점

state는 변경되면 렌더링이 되고, ref는 변경되면 렌더링이 안된다는 차이가 있다.

따라서 아래 함수 `plusStateCountButtonHandler`, `plusRefCountButtonHandler`는 다르게 동작한다.

```jsx
import "./App.css";
import { useRef, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const countRef = useRef(0);

  const plusStateCountButtonHandler = () => {
    setCount(count + 1);
  };

  const plusRefCountButtonHandler = () => {
    countRef.current++;
  };

  return (
    <>
      <div>
        state 영역입니다. {count} <br />
        <button onClick={plusStateCountButtonHandler}>state 증가</button>
      </div>
      <div>
        ref 영역입니다. {countRef.current} <br />
        <button onClick={plusRefCountButtonHandler}>ref 증가</button>
      </div>
    </>
  );
}

export default App;
```

<br>

## 2.2 DOM 접근

<br>
