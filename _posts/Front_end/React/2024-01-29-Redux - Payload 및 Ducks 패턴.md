---
title: "[React] Redux - Payload 및 Ducks 패턴"
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

# 1. Payload 개요

## 1.1 Payload 개념

이전 포스팅에서 counter을 만들었다.

이번 포스팅에서는 더 발전시켜서 +1, -1로 정해진 기능이 아니라 사용자가 직접 증감할 숫자를 정할 수 있게 만들어보자.

input에 5를 입력해서 버튼을 누르면 5가 더해지는 형태의 APP을 만들고 싶으면 Payload를 사용하면 된다.

<br>

> Payload가 정확히 무엇일까?

`“N을 더해”` 라고 N을 같이 리듀서에서 보내야 한다.

지금까지는 ~을 이라는 목적어가 없었다면, 이제는 그 목적어가 생긴것이고 목적어도 액션객체에 담아 같이 보내줘야 할 것이다.

이렇게 액션객체에 같이 담아 보내주는 전달되는 실체를 payload라고 한다. 만약 `10을 더해` 라는 것을 리듀서에게 보내고 싶으면 액션객체에 payload를 같이 담아주는 것 이다.

```jsx
// payload가 추가된 액션객체

{type: "ADD_NUMBER", payload: 10} // type뿐만 아니라 payload라는 key와 value를 같이 담는다.
```

action 객체라는 것은 action type을 payload 만큼 처리하는 것이다. ex) payload가 3이면 3만큼을 plus

<br><br>

# 2. Payload 이용해서 기능 구현하기

## 2.1 사용자가 입력한 값을 받을 input 구현하기

먼저 ui를 만들어보자. input과 button 2개를 아래와 같이 작성하자.

```js
// src/App.js

import React from "react";

const App = () => {
  return (
    <div>
      <input type="number" />
      <button>더하기</button>
      <button>빼기</button>
    </div>
  );
};

export default App;
```

<br>

그 후, input의 값을 state로 관리하기 위해 훅을 사용하여 state 사용하고, 이벤트핸들러 (onChangeHandler)를 작성하여 input과 연결하자.

```jsx
// src/App.js

import React from "react";
import { useState } from "react";

const App = () => {
  const [number, setNumber] = useState(0);

  const onChangeHandler = (event) => {
    const { value } = event.target;
    // event.target.value는 문자열이기 때문에 숫자형으로 형변환하기위해 + 붙힘
    setNumber(+value);
  };

  // 콘솔로 onChangeHandler가 잘 연결되었는지 확인
  console.log(number);

  return (
    <div>
      <input type="number" onChange={onChangeHandler} />
      <button>더하기</button>
      <button>빼기</button>
    </div>
  );
};

export default App;
```

## 2.2 Action Creator 작성하기

counter.js 로 이동해서 모듈을 작성하자.

먼저 작성해야할 모듈을 리스트업 해보자

```jsx
// src/redux/modules/counter.js

// Action Value

// Action Creator

// Initial State

// Reducer

// export default reducer
```

<br>

Action value와 Action Creator를 아래와 같이 작성하자<br>
매개변수 자리에 paylaod를 넣어주자

```jsx
// src/redux/modules/counter.js

// Action Value
const ADD_NUMBER = "ADD_NUMBER";

// Action Creator
export const addNumber = (payload) => {
  return {
    type: ADD_NUMBER,
    payload: payload,
  };
};

// Initial State

// Reducer

// export default reducer
```

## 2.3 리듀서 작성하기

Initial State와 리듀서의 기본 형태를 만들어준 후, 파일의 마지막 부분에 export default 를 통해서 생성한 리듀서를 내보내주자

```jsx
// src/redux/modules/counter.js

// .. 중략

// Initial State
const initialState = {
  number: 0,
};

// Reducer 기본형태
const counter = (state = initialState, action) => {
  switch (action.type) {
    default:
      return state;
  }
};

// export default reducer
export default counter;
```

<br>

그리고 ADD_NUMBER의 로직을 아래와 같이 구현하자

사용자가 컴포넌트에서 Action Creator로 payload를 담아 보내는 것은 액션객체에 담겨지고, 그렇게 담겨진 것은 리듀서에서 action.payload에서 꺼내 사용할 수 있다.

그래서 그것을 이용해서 기존의 값에 더해줌으로써 기능을 구현하는 것이다.

```jsx
// 리듀서

const counter = (state = initialState, action) => {
  switch (action.type) {
    case ADD_NUMBER:
      return {
        // state.number (기존의 nubmer)에 action.paylaod(유저가 더하길 원하는 값)을 더한다.
        number: state.number + action.payload,
      };
    default:
      return state;
  }
};
```

## 2.4 구현된 기능 테스트 하기

UI 및 counter 모듈을 모두 구현했으므로 잘 작동하는지 테스트를 해보자

먼저 App.js에서 useSelector를 이용해서 Store의 값을 조회하고 화면상에 렌더링하는 기능을 추가해보자

```jsx
// src/App.js

import React from "react";
import { useState } from "react";
import { useSelector } from "react-redux";

const App = () => {
  const [number, setNumber] = useState(0);
  const globalNumber = useSelector((state) => state.counter.number);

  const onChangeHandler = (event) => {
    const { value } = event.target;
    setNumber(+value);
  };

  return (
    <div>
      <div>{globalNumber}</div>
      <input type="number" onChange={onChangeHandler} />
      <button>더하기</button>
      <button>빼기</button>
    </div>
  );
};

export default App;
```

<br>

그 후, Action Creator를 import 하고, payload를 담아 dispatch 해보자

```jsx
import React from "react";
import { useState } from "react";
import { useDispatch, useSelector } from "react-redux";

// 4. Action Creatorimport
import { addNumber } from "./redux/modules/counter";

const App = () => {
  // 1. dispatch를 사용하기 위해 선언
  const dispatch = useDispatch();
  const [number, setNumber] = useState(0);
  const globalNumber = useSelector((state) => state.counter.number);

  const onChangeHandler = (event) => {
    const { value } = event.target;
    setNumber(+value);
  };

  // 2. 더하기 버튼을 눌렀을 때 실행할 이벤트핸들러를 만들기
  const onClickAddNumberHandler = () => {
    // 5. Action creator를 dispatch 해주고, 그때 Action creator의 인자에 number를 넣기.
    dispatch(addNumber(number));
  };

  return (
    <div>
      <div>{globalNumber}</div>
      <input type="number" onChange={onChangeHandler} />
      {/* 3. 더하기 버튼 이벤트핸들러를 연결 */}
      <button onClick={onClickAddNumberHandler}>더하기</button>
      <button>빼기</button>
    </div>
  );
};

export default App;
```

잘 작동하는 것을 볼 수 있다!

![](/assets/images/2024/2024-01-30-10-20-14.png)

<br><br>

# 3. Ducks 패턴

Erik Rasmussen 이 제안한 Ducks 패턴은 아래의 내용을 지켜 모듈을 작성하는 것이다.<br>
위에서 작성한 코드들이 Ducks 패턴이다. 자세히 살펴보자.

1. Reducer 함수를 `export default` 한다.

2. Action creator 함수들을 `export` 한다.

3. Action type은 `app/reducer/ACTION_TYPE` 형태로 작성한다.

(외부 라이브러리로서 사용될 경우 또는 외부 라이브러리가 필요로 할 경우에는 UPPER_SNAKE_CASE 로만 작성해도 괜찮다.)

그래서 모듈 파일 1개에 `Action Type`, `Action Creator`, `Reducer` 가 모두 존재하는 작성방식인 것이다.

<br>
