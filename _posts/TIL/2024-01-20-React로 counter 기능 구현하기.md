---
title: "[TIL] React로 counter 기능 구현하기"
categories: [TIL]
tag: [TIL, ToyProject, JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 😹 오늘의 삽질

## 문제점

지금까지 배운 React 문법을 활용하여 간단한 counter 프로그램을 만들어보고자 하였다.

- 조건은 다음과 같았다.

  - `+ 1` 버튼을 누를 때마다 숫자가 + 1 증가한다.
  - `- 1` 버튼을 누를 때마다 숫자가 - 1 감소한다.

  ![](/assets/images/2024/2024-01-21-01-51-34.png)

<br>

정말 쉬운 문제처럼 보였지만, 마음대로 되지 않았다.

처음에는 아래와 같이 작성해주었다.

```jsx
import logo from "./logo.svg";
import "./App.css";
import React, { useState } from "react";

function App() {
  let [number, setNumber] = useState("0");

  const increaseBtnHander = () => {
    setNumber = number++;
  };
  const reductionBtnHander = () => {
    setNumber = number--;
  };

  return (
    <div>
      <div>
        {number}
        <button onClick={increaseBtnHander}>+ 1</button>
        <button onClick={reductionBtnHander}>- 1</button>
      </div>
    </div>
  );
}

export default App;
```

<br>

## 시도해 본 것

- 첫 번째 시도

  `const`는 상수이니 number이 바뀔 수 없는건가? 생각해서 `let`으로 수정해주었다.

  나중에 문제 해결 후 다시 `const`로 변경해주었더니도 작동이 잘 되었다.

  ```jsx
  let increaseBtnHander = () => {
    setNumber = number++;
  };
  let reductionBtnHander = () => {
    setNumber = number--;
  };
  ```

❓ 왜 재할당이 불가능한 상수인 `const`로 써도 작동이 될까?

튜터님한테 물어보고 작성하기

<br>

- 두 번째 시도

두 번째 시도에서는 `setNumber`에 값을 할당하는 것이 아닌 함수를 호출하여 상태를 업데이트해야 한다는 것을 알게 되었다.

useState 훅을 사용하는 경우, useState 함수는 배열을 반환하는데, 이 배열의 첫 번째 요소는 현재 상태의 값이 들어 있는 변수이고, 두 번째 요소는 상태를 업데이트하는 함수이다.

나는 두 번째 요소가 함수라는 사실을 간과한 것이다.

```js
let increaseBtnHander = () => {
  setNumber(number + 1);
};
let reductionBtnHander = () => {
  setNumber(number - 1);
};
```

따라서 위 같이 수정해주었지만 여전히 동작하지 않았다.

![](/assets/images/2024/2024-01-21-02-05-09.png)

<br>

- 세 번째 시도

```js
let increaseBtnHander = () => {
  setNumber(number++);
};
let reductionBtnHander = () => {
  setNumber(number--);
};
```

❓ 작동은 되었지만, 버튼을 두 번 눌러야 증감이 된다는 문제가 있다. 왜 그럴까?

<br>

## 해결방안

정말 바보같았다...

초기값으로 문자열인 "0"을 설정했지만 카운터 기능을 구현하기 위해서는 number 상태를 숫자로 설정해야한다..

```jsx
import "./App.css";
import React, { useState } from "react";

function App() {
  let [number, setNumber] = useState(0);

  let increaseBtnHander = () => {
    setNumber(number + 1);
  };
  let reductionBtnHander = () => {
    setNumber(number - 1);
  };

  return (
    <div
      style={{
        background: "lightgray",
        display: "flex",
        justifyContent: "center",
        alignItems: "center",
        height: "100vh",
        textAlign: "center",
      }}
    >
      <div>
        <div>{number}</div>
        <button onClick={increaseBtnHander}>+ 1</button>
        <button onClick={reductionBtnHander}>- 1</button>
      </div>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-21-02-14-59.png)

<br>

## 👏🏻 잘한점

React의 State와 Hook, Component에 대해 잘 이해하고 있는 것 같다!

어떠한 자료도 참고하지 않고 스스로 해결했기 때문이다.

리액트 배우면 배울수록 너무 재밌는 것 같다..!

빨리 리액트로 팀 프로젝트 하고싶다.!!!

<br>

## ✨ 개선점

초기값을 숫자로 설정하는 것을 놓쳤으며, setNumber 함수를 호출하는 부분을 변수 할당으로 혼동하였다.

- 초기값 설정
  - React에서 상태를 관리하기 위해 useState 훅을 사용할 때, 초기값을 설정해야 하는데, counter의 경우 문자열("0")이 아닌 숫자(0)으로 설정해줬어야 했다.
  - 즉, `const [number, setNumber] = useState(0);`와 같이 초기값을 숫자로 설정해야한다.
    <br><br>
- 변수 할당과 함수 호출
  - setNumber 함수를 호출하는 부분을 변수 할당으로 혼동하였다. useState 훅을 사용하는 경우, useState 함수는 배열을 반환하는데, 이 배열의 첫 번째 요소는 현재 상태의 값이 들어 있는 변수이고, 두 번째 요소는 상태를 업데이트하는 함수이다.
  - 즉,`setNumber = number++;`과 같이 변수를 할당해야하는 것이 아니라 `setNumber(number + 1);`와 같이 setNumber 함수를 호출하여 number 변수를 업데이트해야한다.

<br>

다음에는 이런 자잘한 실수 절대 하지 말자!

<br>
