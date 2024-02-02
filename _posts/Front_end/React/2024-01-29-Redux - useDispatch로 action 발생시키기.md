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
2. State는 상태이며, Reducer는 Swich문을 통해 action의 type에 따라 상태를 제어한다.<br>
3. UI(컴포넌트)에서 이벤트가 발생해서 store(중앙 데이터 관리소)에 있는 state를 바꿔야하는 상황이면(ex. onClick)<br> Dispatch가 스토어에게 <span style="color:indianred">액션 객체(type, payload)</span>를 전달한다. (Dispatch는 액션 객체를 가지고 스토어에게 던져주는 느낌⭐)
4. 스토어의 Reducer은 액션 객체에 있는 type에 따라 state를 변경한다.<br> → 스토어에 있는 state에 직접 접근하는 것이 금지되어 있기 때문

<br><br>

# 2. state 수정 기능 만들기

counter.js 모듈의 state 수정 기능을 만들어보자. (카운터 기능 구현)

> 어떻게 counter.js 모듈에 있는 state의 값을 변경할 수 있을까?

useState()가 아닌 리덕스를 사용했을 때는 액션객체를 만들어 스토어에게 전달하는 과정을 거쳐야한다. 리덕스에서 값의 수정은 스토어의 리듀서에서 일어나기 때문이다.

리듀서란, 디스패치를 통해 전달받은 액션객체를 검사하고, 조건이 일치했을 때 새로운 상태값을 만들어내는 "변화를 만들어내는" 함수이다.

1. 스토어에있는 리듀서에게 보낼 액션객체를 만든다. (액션객체는 반드시 type이라는 `key`를 가져야 한다.)
2. 디스패치를 사용해서 액션객체를 리듀서로 보낸다.(디스패치를 사용하기위해 `useDispatch()` 라는 훅을 사용해야한다. )
3. 리듀서에서 명령을 받아 number +1을 한다.

<br>

![리듀서 흐름 이해](../../../assets/images/2024/%EB%A6%AC%EB%93%80%EC%84%9C%ED%9D%90%EB%A6%84%EC%9D%B4%ED%95%B4.jpg)

위 그림처럼 Reducer + Action 파일을 분리하지 않고 같이 두었다.<br>
이를 모듈이라 부를 것이다. 코드를 통해 자세히 살펴보자.

<br>

> App.jsx

```jsx
// src/App.jsx

import { useDispatch, useSelector } from "react-redux";

// 사용할 Action creator를 import
import { plusOne, minusOne } from "./redux/modules/counter";

const App = () => {
  // useDispatch를 통해 store부터 dispatch 함수를 가져온다.
  const dispatch = useDispatch();
  // useSelector로 store의 state 값을 불러올 수 있다.
  const date = useSelector((state) => state.counter);

  return (
    <>
      <div>현재 카운트: {date.number}</div>
      <div>
        {/* 버튼 클릭시 해당 액션 디스패치 */}
        <button
          onClick={() => {
            // dispatch 함수는 액션을 파라미터로 전달한다.
            dispatch(plusOne()); // 액션객체를 Action creator로 변경
          }}
        >
          + 1
        </button>
      </div>
      <div>
        <button
          onClick={() => {
            dispatch(minusOne());
          }}
        >
          - 1
        </button>
      </div>
      <div>{/* 합계 : {totalNumber} */}</div>
    </>
  );
};

export default App;
```

<br>

> counter.js(모듈)

```jsx
// src/redux/modules/counter.js

// 액션객체 type의 value를 상수들로 만들어 준다. (상수로 만드는 이유: 휴먼에러 방지)
// 액션객체 type의 value는 대문자로 작성한다.
const PLUS_ONE = "PLUS_ONE";
const MINUS_ONE = "MINUS_ONE";

// Action Creator 생성 (액션객체를 만드는 함수)
// Action Creator는 모듈 파일안에서 생성
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

// 초기 상태값
const initialState = {
  number: 0,
};

// 리듀서(현재상태 + 액션객체(type, payload) => 새로운 상태 반환하는 "함수")
const counter = (state = initialState, action) => {
  //  switch 문을 사용하여 액션의 타입에 따라 다른 로직을 수행하고 상태를 업데이트
  switch (action.type) {
    case PLUS_ONE: // case에서도 문자열이 아닌, 위에서 선언한 상수를 넣어준다.
      return {
        ...state, // 다른 상태값을 유지하기 위해 현재 상태를 복사한다.
        number: state.number + 1,
      };

    case MINUS_ONE:
      return {
        ...state,
        number: state.number - 1,
      };

    default:
      return state;
  }
};

export default counter;
```

잘 작동한다!

![](/assets/images/2024/2024-02-02-10-39-06.png)

<br>
