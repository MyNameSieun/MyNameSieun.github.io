---
title: "[React] State와 Hook"
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

# 1. State 개요

## 1.1 State 개념

State(상태)란 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다.

React에선 `let`이나 `const`가 아닌 `State`를 사용해서 상태를 표현한다.

왜 변수가 아닌 State를 사용할까? 바로<span style="color:indianred"> UI(엘리먼트)를 바꾸기 위해서</span>이다.

전 포스팅에서 name이라는 정보를 `const name = “홍부인”;` 이라고 만들었는데, 만약 name이라는 값이 바뀌어야만 하는 정보였어야 했다면 `state`로 생성하는 것이다!

<br>

## 1.2 State 만들기

① State를 만들 때는 useState()를 사용한다.

② React 안에서 특정한 기능을 수행하는 것들을 hook이라고 부른다. (기능 → 훅)

③ useState 훅을 사용하는 방식은 아래와 같다.

```js
// useState hook
useState("initial State") = const [state, setState] = ;
```

- state: 현재 상태의 값이 들어 있는 변수
- setState: 상태를 업데이트하는 함수
- initialState: 초기 상태 값을 지정

<br>

> 위 코드를 구조분해할당해주면 아래와 같다.📌

```js
// useState hook
const [state, setState] = useState("initial State");
```

- 먼저 `const` 로 선언을 하고 `[ ] 빈 배열` 을 생성하고, 배열의 첫번째 자리에는 이 state의 이름, 그리고 두번째 자리에는 `set` 을 붙이고 state의 이름을 붙힌다.

- 그리고 `useState( )` 의 인자에는 이 state의 `원하는 초기값` 을 넣어준다.

<br>

> 컴포넌트 내에서 숫자의 증감을 컨트롤할 수 있는 counter이라는 state를 만들어보자

```js
// useState hook
const [count, setCount] = useState[0];
const [todoList, setTodoList] = useState([]);
```

<br>

> State 만들기 예시

```jsx
// src/App.js

import React, { useState } from "react";

function Child(props) {
  return <div>{props.grandFatherName}</div>;
}

function Mother(props) {
  return <Child grandFatherName={props.grandFatherName} />;
}

function GrandFather() {
  const [name, setName] = useState("김할아"); // state를 생성
  return <Mother grandFatherName={name} />;
}

function App() {
  return <GrandFather />;
}

export default App;
```

`name` 이라는 state를 만들었고, name state의 초기값은 “김할아”로 정했다.

이처럼 초기값을 `initial state` 라고 부른다.

state의 정의처럼, 언제든지 변할 수 있는 값이기 때문에 `초기값` 이라는 개념이 존재하는 것이다.

<br><br>

# 2. State 변경하기

state를 변경할때는 `setValue(바꾸고 싶은 값)` 를 사용한다.

<span style="color:indianred"> state란 컴포넌트안에서 변할 수 있는 값</span> 이라는 것을 기억하자!

<br>

## 2.1 useState + onClick Event

> Button과 이벤트 핸들러 구현하기

버튼을 클릭했을 때 state가 변경하도록 만들기 위해 `<button>` 태그를 이용해서 버튼을 생성하자.

```jsx
// src/App.js

import React from "react";

function App() {
  return (
    <div>
      이름
      <br />
      <button>버튼</button>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-18-16-19-06.png)

<br>

이름을 변경할 수 있도록 해야하기 때문에 이름을 state로 선언한다.

```js
import { useState } from "react";

function App() {
  const [name, setName] = useState("박시은");

  return (
    <div>
      {name}
      <br />
      <button>버튼</button>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-18-16-26-07.png)

<br>

그 후, 버튼을 누르면 이름이 변경되도록 해보자.

```js
function App() {
  const [name, setName] = useState("박시은");

  return (
    <div>
      {name}
      <br />
      <button
        onClick={function () {
          setName("시은천사");
        }}
      >
        버튼
      </button>
    </div>
  );
}
```

![](/assets/images/2024/2024-01-18-16-29-48.png)

<br>

아래와 같이 이벤트 핸들러를 사용할 수 있다.

```js
import { useState } from "react";

function App() {
  const [name, setName] = useState("박시은");

  function onClickHandler() {
    setName("시은천사");
  }

  return (
    <div>
      {name}
      <br />
      <button onClick={onClickHandler}>버튼</button>
    </div>
  );
}

export default App;
```

<br>

## 2.2 useState + onChange Event

> input 태그 안에 값을 입력했을 때 컨텐츠가 변경되도록 해보자.

input에서는 보통 사용자가 입력한 값을 state로 관리하는 패턴을 많이 사용한다.

input태그 안에 value와 onChange()를 넣어주면 된다.

```jsx
import { useState } from "react";

function App() {
  const [fruit, setFruit] = useState("");

  return (
    <div>
      과일:
      <input
        value={fruit}
        onChange={function (event) {
          setFruit(event.target.value);
        }}
      />
    </div>
  );
}

export default App;
```

위 코드처럼 이벤트 핸들러 안에서 자바스크립트의 event 객체를 꺼내 사용할 수 있다.

사용자가 입력한 input의 값은 `event.target.value` 로 꺼내 사용할 수 있다.

정리하자면, 사용자가 input에 어떤 값을 입력하면, 그 값을 입력할 때마다, 같은 말로 onChange될 때마다 value라는 state에 setValue해서 넣어주는 것이다.

<br><br>

# 3. 실습하기

> 아이디와 비밀번호에 값을 입력하고 [로그인] 버튼을 누르면 alert로 고객이 입력한 값을 알려주자.

- 먼저 아래와 같은 화면을 만들어 보자<br><br>
  ![](/assets/images/2024/2024-01-18-16-59-14.png)
  <br> <br>
- 그후, 다음과 같이 alert의 메세지 내용을 출력하자
  - "고객님이 입력하신 아이디는 '입력아이디' 이며, 비밀번호는 '입력비밀번호' 입니다."
    <br><br>
- 아래의 사항을 준수하자.<br>
  - (1) id 필드와 비밀번호 필드의 값을 state로 관리하고, 변경이 일어날 때마다 setState를 해서 동기화를 시켜주자.
  - (2) alert로 띄우고 나서는 아이디와 비밀번호 영역을 빈 값으로 초기화 시켜주자.
  - (3) 비밀번호는 보이면 안된다.

<br>

---

<br>

먼저 아래와 같이 html을 만들어두자

```jsx
function App() {
  const [fruit, setFruit] = useState("");

  return (
    <div>
      <div>
        아이디 : <input type="text"></input>
      </div>
      <div>
        비밀번호 : <input type="password"></input>
      </div>
      <button>로그인</button>
    </div>
  );
}
```

<br>

id 필드와 비밀번호 필드의 값을 state로 관리하고, 변경이 일어날 때마다 setState를 해서 동기화를 시킨 후 alert로 띄워주자.

```js
import { useState } from "react";

function App() {
  const [id, setId] = useState("");
  const [password, setPassword] = useState("");

  // console.log("id", id);
  // console.log("password", password);

  // id 필드가 변경될 경우
  const onIdChangeHandler = (event) => {
    setId(event.target.value);
  };

  // password 필드가 변경될 경우
  const onPasswordChangeHandler = (event) => {
    setPassword(event.target.value);
  };

  return (
    <div>
      <div>
        아이디 : <input type="text" value={id} onChange={onIdChangeHandler}></input>
      </div>
      <div>
        비밀번호 :{" "}
        <input
          type="password"
          value={password}
          onChange={onPasswordChangeHandler}
        ></input>
      </div>
      <button
        onClick={() => {
          alert(
            `고객님이 입력하신 아이디는 ${id}이며, 비밀번호는 ${password}입니다.`
          );
        }}
      >
        로그인
      </button>
    </div>
  );
}

export default App;
```

![](/assets/images/2024/2024-01-18-17-23-32.png)

<br>

아이디와 비밀번호 영역을 빈 값으로 초기화 시켜주자.

아래와 같이 state 값을 초기화 시켜주면 된다.

```js
setId("");
setPassword("");
```

```jsx
<button
  onClick={() => {
    alert(
      `고객님이 입력하신 아이디는 ${id}이며, 비밀번호는 ${password}입니다.`
    );
    setId("");
    setPassword("");
  }}
>
  로그인
</button>
```

<br>
