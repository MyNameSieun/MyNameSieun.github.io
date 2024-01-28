---
title: "[React] React Hooks - 최적화(React.memo, useCallback, useMemo)"
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

# 1. 렌더링과 최적화

## 1.1 리-렌더링의 발생 조건

1. 컴포넌트에서 state가 바뀌었을 때
2. 컴포넌트가 내려받은 props가 변경되었을 때
3. 부모 컴포넌트가 리-렌더링 된 경우 자식 컴포넌트는 모두

<br>

## 1.2 최적화

리액트에서 리렌더링을 줄이는 작업을 최적화(Optimization)라고 부른다.

리액트에서 불필요한 렌더링이 발생하지 않도록 최적화하는 대표적인 방법은 아래와 같다.

1. memo(React.memo) : 컴포넌트를 캐싱
2. useCallback : 함수를 캐싱
3. useMemo : 값을 캐싱

<br><br>

# 2.memo(React.memo) 개요

## 2.1 memo 개념

리-렌더링의 발생 조건 중 3번째 경우. 즉, 부모 컴포넌트가 리렌더링 되면 자식컴포넌트는 모두 리렌더링 된다는 것은 그림으로 보면 아래와 같다.

![](/assets/images/2024/2024-01-27-20-32-30.png)

- 1번 컴포넌트가 리렌더링 된 경우, 2~7번이 모두 리렌더링 된다.
- 4번 컴포넌트가 리렌더링 된 경우, 6, 7번이 모두 리렌더링 된다.

따라서 자식 컴포넌트는 변경 사항이 없음에도 렌더링되는 문제가 발생한다. 이를 memo를 통해 해결할 수 있다.

<br>

## 2.2 memo 사용x

아래와 같이 디렉토리를 구성해보자

![](/assets/images/2024/2024-01-27-20-58-28.png)

그 후, 예제 코드를 만들어보자.

```jsx
// App.jsx
import React, { useState } from "react";
import Box1 from "./components/Box1";
import Box2 from "./components/Box2";
import Box3 from "./components/Box3";

const boxesStyle = {
  display: "flex",
  marginTop: "10px",
};

function App() {
  console.log("App 컴포넌트가 렌더링되었습니다!");

  const [count, setCount] = useState(0);

  // 1을 증가시키는 함수
  const onPlusButtonClickHandler = () => {
    setCount(count + 1);
  };

  // 1을 감소시키는 함수
  const onMinusButtonClickHandler = () => {
    setCount(count - 1);
  };

  return (
    <>
      <h3>카운트 예제입니다!</h3>
      <p>현재 카운트 : {count}</p>
      <button onClick={onPlusButtonClickHandler}>+</button>
      <button onClick={onMinusButtonClickHandler}>-</button>
      <div style={boxesStyle}>
        <Box1 />
        <Box2 />
        <Box3 />
      </div>
    </>
  );
}

export default App;
```

```jsx
// Box1.jsx
import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#91c49f",
  color: "white",

  // 가운데 정렬 3종세트
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function Box1() {
  console.log("Box1이 렌더링되었습니다.");
  return <div style={boxStyle}>Box1</div>;
}

export default Box1;
```

```jsx
// Box2.jsx
import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#4e93ed",
  color: "white",

  // 가운데 정렬 3종세트
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function Box2() {
  console.log("Box2가 렌더링되었습니다.");
  return <div style={boxStyle}>Box2</div>;
}

export default Box2;
```

```jsx
// Box3.jsx
import React from "react";

const boxStyle = {
  width: "100px",
  height: "100px",
  backgroundColor: "#c491be",
  color: "white",

  // 가운데 정렬 3종세트
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

function Box3() {
  console.log("Box3가 렌더링되었습니다.");
  return <div style={boxStyle}>Box3</div>;
}

export default Box3;
```

![](/assets/images/2024/2024-01-27-20-57-45.png)

plus 버튼 또는 minus 버튼을 누른 순간 실제로 변한 것은 부모컴포넌트, App.jsx 뿐인데 모든 하위 컴포넌트가 리렌더링 되고 있다는 것을 알 수 있다.

![](/assets/images/2024/2024-01-27-21-03-25.png)

이런 현상을 memo를 통해 해결할 수 있는 것이다.

<br>

## 2.3 memo 사용o

`React.memo`를 이용하면 컴포넌트를 메모리에 저장(캐싱)해두고 필요할 때 갖다 쓸 수 있다.

이렇게 하면 부모 컴포넌트의 `state`의 변경으로 인해 `props`가 변경이 일어나지 않는 한 컴포넌트는 리렌더링 되지 않는다.

이것을 컴포넌트 memoization 이라고 한다.

<br>

사용법은 정말 간단하다.

Box1.jsx, Box2.jsx, Box3.jsx 모두 동일하지만 export 부분만 아래와 같이 바꿔주면 된다.

```jsx
export default React.memo(Box1);
export default React.memo(Box2);
export default React.memo(Box3);
```

![](/assets/images/2024/2024-01-27-21-09-40.png)

<br><br>

# 3. useCallback 개요

## 3.1 useCallback 개념

<br>

## 3.2 useCallback 사용x

<br>

## 3.3 useCallback 사용o

<br><br>

# 4. useMemo 개요

## 4.1 useMemo 개념

<br>

## 4.2 useMemo 사용x

<br>

## 4.3 useMemo 사용o

<br>
