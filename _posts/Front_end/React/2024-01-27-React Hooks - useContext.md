---
title: "[React] React Hooks - React.memo"
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

# 1. useContext 개요

## 1.1 react context 등장 배경

일반적으로 부모컴포넌트 → 자식 컴포넌트로 데이터를 전달해 줄 때 props를 사용하였다. 하지만, 이 방식으로 데이터를 전달했을 때 prop drilling 현상이 일어난다는 문제점이 있다.

> prop drilling의 문제점

1. 깊이가 너무 깊어지면 prop이 어떤 컴포넌트로부터 왔는지 파악이 어려워진다.
2. 어떤 컴포넌트에서 오류가 발생할 경우 추적이 힘들어지니 대처가 늦을 수 밖에 없다.<br><br>
   ![](/assets/images/2024/2024-01-27-14-51-47.png)
   자료 : https://www.copycat.dev/blog/react-context/

따라서 이러한 문제를 해결하기 위해 react context API가 등장하게 되었다. useContext hook을 통해 쉽게 전역 데이터를 관리할 수 있다.

<br>

## 1.2 context API 필수 개념

- `createContext` : context 생성
- `Consumer` : context 변화 감지
- `Provider` : context 전달(to 하위 컴포넌트)

<br><br>

# 2. useContext 구현하기

## 2.1 useContext 사용x

![](/assets/images/2024/2024-01-27-18-38-16.png)

```jsx
// App.jsx
import "./App.css";
import GrandFather from "./components/GrandFather";
export function App() {
  return <GrandFather />;
}

export default App;
```

```jsx
// GrandFather.jsx
import React from "react";
import Father from "./Father";

function GrandFather() {
  const houseName = "박하우스";
  const pocketMoney = 10000;

  return <Father houseName={houseName} pocketMoney={pocketMoney} />;
}

export default GrandFather;
```

```jsx
// Father.jsx
import React from "react";
import Child from "./Child";

function Father({ houseName, pocketMoney }) {
  return <Child houseName={houseName} pocketMoney={pocketMoney} />;
}

export default Father;
```

```jsx
// Child.jsx
import React from "react";

function Child({ houseName, pocketMoney }) {
  const stressedWord = {
    color: "red",
    fontWeight: "900",
  };
  return (
    <div>
      나는 이 집안의 막내에요.
      <br />
      할아버지가 우리 집 이름은 <span style={stressedWord}>{houseName}</span>
      라고 하셨어요.
      <br />
      게다가 용돈도 <span style={stressedWord}>{pocketMoney}</span>원만큼이나 주셨답니다.
    </div>
  );
}

export default Child;
```

![](/assets/images/2024/2024-01-27-18-44-35.png)

위 코드에서 `GrandFather` 컴포넌트는 `Child` 컴포넌트에게 **houseName**과 **pocketMoney**를 전달해주기 위해 `Father` 컴포넌트를 거칠 수 밖에 없었다.

`useContext` hook을 적용해서 효율적으로 바꿔보자

<br>

## 2.2 useContext 사용o

![](/assets/images/2024/2024-01-27-18-46-22.png)

```jsx
// context/FamilyContext.js
import { createContext } from "react";

export const FamilyContext = createContext(null);
```

```jsx
// GrandFather.jsx
import React from "react";
import Father from "./Father";
import { FamilyContext } from "../context/FamilyContext";

function GrandFather() {
  const houseName = "박하우스";
  const pocketMoney = 10000;

  return (
    // FamilyContext를 import주고 그 안에 컴포넌트 넣어주기
    <FamilyContext.Provider value={{ houseName, pocketMoney }}>
      <Father /> {/* props 삭제 */}
    </FamilyContext.Provider>
  );
}

export default GrandFather;
```

```jsx
// Father.jsx
import React from "react";
import Child from "./Child";

function Father() {
  // props 제거
  return <Child />;
}

export default Father;
```

```jsx
// Child.jsx
import React, { useContext } from "react";
import { FamilyContext } from "../context/FamilyContext";

const stressedWord = {
  color: "red",
  fontWeight: "900",
};

function Child() {
  const data = useContext(FamilyContext);
  console.log("data", data);

  return (
    <div>
      나는 이 집안의 막내에요.
      <br />
      할아버지가 우리 집 이름은{" "}
      <span style={stressedWord}>{data.houseName}</span>
      라고 하셨어요.
      <br />
      게다가 용돈도 <span style={stressedWord}>{data.pocketMoney}</span>
      원만큼이나 주셨답니다.
    </div>
  );
}

export default Child;
```

<br>

## 2.3 주의사항

> 렌더링 문제

useContext를 사용할 때, Provider에서 제공한 value가 달라진다면 useContext를 사용하고 있는 모든 컴포넌트가 리렌더링 된다.

따라서 value 부분을 항상 신경써줘야 한다.

따라서 메모이제이션이 필요하다.

<br>
