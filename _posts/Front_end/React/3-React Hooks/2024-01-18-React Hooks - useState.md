---
title: "[React] React Hooks - useState"
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

전 포스팅에서 name이라는 정보를 `const name = "김춘자";` 이라고 만들었는데, 만약 name이라는 값이 바뀌어야만 하는 정보였어야 했다면 `state`로 생성하는 것이다!

<br>

## 1.2 State 만들기

① State를 만들 때는 useState()를 사용한다.

② React 안에서 특정한 기능을 수행하는 것들을 hook이라고 부른다. (기능 → 훅)

⬇️ useState 훅을 사용하는 방식은 아래와 같다.

```js
// state: 현재 상태의 값이 들어 있는 변수
// setState: 상태를 업데이트하는 함수
//  initialState: 초기 상태 값을 지정
const [state, setState] = useState("initial State");
```

- 먼저 `const` 로 선언을 하고 `[ ] 빈 배열` 을 생성하고, 배열의 첫번째 자리에는 이 state의 이름, 그리고 두번째 자리에는 `set` 을 붙이고 state의 이름을 붙힌다.

- 그리고 `useState( )` 의 인자에는 이 state의 `원하는 초기값` 을 넣어준다.

<br>

> 컴포넌트 내에서 숫자의 증감을 컨트롤할 수 있는 counter이라는 state를 만들어보자

```js
// useState hook
const [count, setCount] = useState[0];
```

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
// src/App.js
import React, { useState } from "react";

function App() {
  const [name, setName] = useState("박시은");

  return (
    <div>
      <div>{name}</div>
      <button
        onClick={function () {
          setName("시은 천사");
        }}
      >
        버튼
      </button>
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

> 아래 그림과 같이 input 태그 안에 값을 입력했을 때 컨텐츠가 변경되도록 해보자.

![](/assets/images/2024/2024-01-20-23-41-46.png)

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
  const [id, setId] = useState("");

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

<br><br>

# 4. 함수형 업데이트 개요

## 4.1 함수형 업데이트 사용법

> 함수형 업데이트 방식을 통해 setState를 사용할 수 있다.

```js
// 일반 사용법(기존 방식)
setState(number + 1);

// 함수형 업데이트
setState(() => {});
```

① setState의 ( ) 안에 수정할 값이 아니라, 함수를 넣을 수 있다.<br>
② 그 함수의 인자에서는 현재의 state을 가져올 수 있고, { } 안에서는 이 값을 변경하는 코드를 작성할 수 있다.

<br>

```js
// 현재 number의 값을 가져와서 그 값에 +1을 더하여 반환
setState((currentNumber) => {
  return currentNumber + 1;
});
```

<br>

## 4.2 함수형 업데이트 필요성(with 배치)

> 일반 사용법과 함수형 업데이트 방식의 차이점

일반 업데이트 방식으로 onClick안에서 setNumber(number + 1) 를 3번 호출 → number가 1씩 증가

```jsx
// src/App.js

import { useState } from "react";

const App = () => {
  const [number, setNumber] = useState(0);
  return (
    <div>
      {/* 버튼을 누르면 1씩 플러스된다. */}
      <div>{number}</div>
      <button
        onClick={() => {
          setNumber(number + 1); // 첫번째 줄
          setNumber(number + 1); // 두번쨰 줄
          setNumber(number + 1); // 세번째 줄
        }}
      >
        버튼
      </button>
    </div>
  );
};

export default App;
```

① 일반 업데이트 방식은 버튼을 클릭했을 때 첫번째 줄 ~ 세번째 줄의 있는 setNumber가 각각 실행되는 것이 아니라, <span style="color:indianred">배치(batch)로 처리⭐</span>한다.

② 즉 onClick을 했을 때 setNumber 라는 명령을 몇 번을 내리던지 간에, 리액트는 그 명령을 하나로 모아 최종적으로 한번만 실행시킨다.

③ 따라서 setNumber을 3번 명령하던, 100번 명령하던 1번만 실행된다.

<br>

> 함수 업데이트 방식으로 onClick안에서 setNumber(number + 1) 를 3번 호출 → number가 3씩 증가

```jsx
// src/App.js

import { useState } from "react";

const App = () => {
  const [number, setNumber] = useState(0);
  return (
    <div>
      {/* 버튼을 누르면 3씩 플러스 된다. */}
      <div>{number}</div>
      <button
        onClick={() => {
          setNumber((previousState) => previousState + 1);
          setNumber((previousState) => previousState + 1);
          setNumber((previousState) => previousState + 1);
        }}
      >
        버튼
      </button>
    </div>
  );
};

export default App;
```

① 함수형 업데이트 방식은 3번을 동시에 명령을 내리면, 그 명령을 모아 순차적으로 각각 1번씩 실행시킨다.

② 0에 1더하고, 그 다음 1에 1을 더하고, 2에 1을 더해서 3을 출력한다.

③ 이는 불필요한 리-렌더링을 방지(렌더링 최적화)하여 리액트의 성능을 향상시키기 위해 단일 업데이트(batch update)로 한꺼번에 처리할 수 있게 하는 것이다.

<br>

## 4.2 함수형 업데이트의 문제점

리액트에서 상태 업데이트는 비동기적으로 이루어지기 때문에 상태가 즉시 업데이트 되지 않는다.

따라서 아래와 같은 문제가 발생하게 된다.

```js
import { useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    // 첫 번째 상태 업데이트
    setCount(count + 1);
    console.log(count); // 아직 상태 변경 x, 초기 상태가 0이므로 0 출력 (비동기적으로 작동하기 때문)

    // 두 번째 상태 업데이트
    setCount(count + 1);
    console.log(count); // 아직 상태 변경 x, 이전 상태의 값 1 출력 (비동기적으로 작동하기 때문)
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increase</button>
    </div>
  );
}

export default App;
```

setCount 호출에서는 각각 현재 상태가 아닌 이전 상태를 기반으로 하고 있기 때문에, 예상대로 두 번의 클릭에도 불구하고 상태가 1만 증가하게된다.<br><br>
![](/assets/images/2024/2024-01-31-15-37-53.png)

<br>

## 4.3 함수형 업데이트 문제 해결방안

"useEffect + 함수형 업데이트 사용"을 하면 함수형 업데이트 문제(배치 업데이트로 업데이트 값이 즉시 반영되지 않는 문제)를 해결할 수 있다.

```js
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("useEffect");
    console.log(count);
  }, [count]);

  function handleClick() {
    // 첫 번째 상태 업데이트
    setCount((prev) => prev + 1);
    console.log(count); // 아직 상태 변경 x (비동기적으로 작동하기 때문)

    // 두 번째 상태 업데이트
    setCount((prev) => prev + 1);
    console.log(count); // 아직 상태 변경 x (비동기적으로 작동하기 때문)
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increase</button>
    </div>
  );
}

export default App;
```

useEffect에 대해선 다음 포스팅에서 자세히 다룰 것이다!

<br>
