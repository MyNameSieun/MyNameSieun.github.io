---
title: "[React] Redux - useSelector로 스토어의 state 조회하기"
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

useState 로 만들었던 카운터 프로그램을 리덕스로 다시 만들어보자

# 1. 모듈

## 1.1 모듈 만들기

모듈이란, State의 그룹이다.

카운터 프로그램에 필요한 State들이 모여있는 모듈을 만들어보자.

1. modules 폴더에 counter.js 파일을 생성
2. 코드 작성

   ```js
   // src/redux/modules/counter.js

   // (1) 초기 상태값
   const initialState = {
     number: 0,
   };

   // (2)리듀서(state에 변화를 일으키는 "함수")

   // state를 action의 type에 따라 변경하는 함수
   // input: state와 action
   const counter = (state = initialState, action) => {
     switch (action.type) {
       default:
         return state;
     }
   };

   // 모듈파일에서는 리듀서를 export default 한다.
   export default counter;
   ```

<br>

> (1) initialState === 초기 상태값

useState를 사용했을 때 괄호 안에 초기값을 지정해주던 것과 같다.

```js
// useState() 사용

const [number, setNumber] = useState(0); // < 초기값 지정
```

<br>

> (2) Reducer

useState()를 사용할 때, number라는 값을 바꾸고 싶으면 아래 코드처럼 setNumber를 사용하여 값을 변경할 수 있었다.

리덕스에서는 리듀서가 이 역할을 한다.

```js
// useState() 사용

const onClickHandler = () => {
  setNumber(number + 1); // setState를 이용해서 state 변경
};
```

리듀서의 인자에 보면 `(state = intialState, action)` 이라고 되어있다.

1. `state = intialState` 처럼 state에 initialState를 할당해줘야한다.
2. 리듀서 인자 첫번째 자리에서는 state를, 두번째 자리에서는 action 이라는 것을 꺼내서 사용할 수 있다.

<br>

# 2. 카운터 모듈을 스토어에 연결하기

modules/couter.js에서 만든 리듀서를 rootReducer 안에 넣어주면 된다.

```jsx
// src/redux/config/configStore.js
import { createStore } from "redux";
import { combineReducers } from "redux";
import counter from "../modules/counter"; // (1) counter reducer import

const rootReducer = combineReducers({
  // (2) reduce 넣어 사용하기
  counter,
});
const store = createStore(rootReducer);

export default store;
```

<br>

원래는 컴포넌트 내에서 state를 만들었기 때문에 그냥 state에 접근하면 됐지만, redux 사용시 중앙 저장소 store에 state(counter)가 존재하므로 store에 접근하는 로직이 필요하다.

![](/assets/images/2024/2024-01-29-15-41-09.png)

<br>

```js
// src/redux/modules/counter.js
import logo from "./logo.svg";
import "./App.css";
import { useSelector } from "react-redux";

function App() {
  // 가정: 여기서 store에 접근하여. counter의 값을 읽어오고 싶다.
  // → useSelector Hook 사용 : redux store의 state를 가져옴
  const date = useSelector((state) => {
    // 매개변수로 들어가는 state는 store 전체의 state를 말한다.
    //화살표 함수의 인자에서 값을 꺼내 return
    return state;
  });
  console.log(date);

  {
    /* 가져온 state를 사용하여 컴포넌트를 렌더링 */
  }
  return <div>{date}</div>;
}

export default App;
```

브라우저를 켜고, 콘솔을 보면 아래 이미지처럼 객체가 보이고, 그 안에 counter 라는 값이 있는 것을 볼 수 있다.

즉, 만든 counter 라는 모듈의 state가 보이는 것이다.

![](/assets/images/2024/2024-01-29-15-26-26.png)

<br>

이제 number라는 값을 사용하고자 한다면 어떤 컴포넌트에서던지 아래 코드처럼 꺼내서 사용하면 된다.

```js
function App() {
  const date = useSelector((state) => {
    return state.counter;
  });
  console.log("counter:", date.number);

  return <div>{date}</div>;
}
```

아래와 같이 사용할 수도 있다.

```js
function App() {
  const date = useSelector((state) => state.counter.number); // 0
  console.log("counter:", date);

  return <div>{date}</div>;
}
```

![](/assets/images/2024/2024-01-29-15-30-55.png)

<br>

> 정리

- ⭐⭐ 컴포넌트에서 리덕스 스토어를 조회하고자 할 때는 <span style="color:indianred">useSelector라는 ‘react-redux’의 훅을 사용</span>해야 한다.
- useSelector는 리덕스 스토어의 상태(state)를 조회한다.
- 리덕스는 상태 관리 라이브러리로서 중앙 집중식 상태를 관리하는데, useSelector를 사용하면 리액트 컴포넌트에서 리덕스 스토어에 저장된 상태를 가져와 컴포넌트에서 사용할 수 있게 해주는 역할을 합니다.
- useSelector의 인자로 전달되는 콜백 함수는 현재 리덕스 스토어의 전체 상태를 받아오며, 여기서 필요한 부분만 선택하여 반환한다. 즉, 이 콜백 함수는 전체 스토어 상태에서 필요한 부분만 추출하는데 사용된다.

<br>
