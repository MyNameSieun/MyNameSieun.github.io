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
3. useMemo : 값을 캐싱 (함수가 리턴하는 값, 값 자체)

<br><br>

# 2.memo(React.memo) 개요

## 2.1 memo 개념

리-렌더링의 발생 조건 중 3번째 경우. 즉, 부모 컴포넌트가 리렌더링 되면 자식컴포넌트는 모두 리렌더링 된다는 것은 그림으로 보면 아래와 같다.

![](/assets/images/2024/2024-01-27-20-32-30.png)

- 1번 컴포넌트가 리렌더링 된 경우, 2~7번이 모두 리렌더링 된다.
- 4번 컴포넌트가 리렌더링 된 경우, 6, 7번이 모두 리렌더링 된다.

따라서 자식 컴포넌트는 변경 사항이 없음에도 렌더링되는 문제가 발생한다. 이를 memo를 통해 해결할 수 있다.

<br>

## 2.2 memo의 필요성

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

## 2.3 memo 적용

`React.memo`를 이용하면 컴포넌트를 메모리에 저장(캐싱)해두고 필요할 때 갖다 쓸 수 있다.

이렇게 하면 부모 컴포넌트의 `state`의 변경으로 인해 `props`가 변경이 일어나지 않는 한 컴포넌트는 리렌더링 되지 않는다. 즉, props로 전달받은 자식 컴포넌트가 리렌더링 되지 않게 하기 위해 memo를 사용하는 것이다.

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

React.memo는 컴포넌트를 메모이제이션 했다면, useCallback은 인자로 들어오는 함수 자체를 기억(메모이제이션)한다.

<br>

## 3.2 useCallback의 필요성

아래와 같이 Box1이 count를 초기화 해 주는 코드라고 가정해보자.

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

  // counter를 초기화해주는 함수
  const initCount = () => {
    setCount(0);
  };

  return (
    <>
      <h3>카운트 예제입니다!</h3>
      <p>현재 카운트 : {count}</p>
      <button onClick={onPlusButtonClickHandler}>+</button>
      <button onClick={onMinusButtonClickHandler}>-</button>
      <div style={boxesStyle}>
        <Box1 initCount={initCount} />
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

function Box1({ initCount }) {
  console.log("Box1이 렌더링되었습니다.");
  return (
    <div style={boxStyle}>
      <button onClick={() => initCount()}>초기화</button>
    </div>
  );
}

export default React.memo(Box1);
```

![](/assets/images/2024/2024-01-29-01-08-15.png)

`+` 버튼이나, `-` 버튼을 누를 때 그리고 초기화 버튼을 누를 때 모두 App 컴포넌트와 Box1 컴포넌트가 리렌더링 되는 것을 볼 수 있다.

![](/assets/images/2024/2024-01-29-01-08-48.png)

<br>

왜 React.memo를 통해서 Box1.jsx는 메모이제이션을 했는데도 리렌더링이 될까?

바로 우리가 함수형 컴포넌트를 사용하기 때문에 App.jsx가 리렌더링 되면서 코드가 다시 만들어지기 때문이다.

```jsx
const onInitButtonClickHandler = () => {
  initCount();
};
```

<br>

자바스크립트에서는 함수도 객체의 한 종류이기때문에, 코드의 변경 사항이 없더라도 다시 만들어지면 그 <span style="color:indianred">주솟값이 달라지기 때문에</span> 하위 컴포넌트인 Box1.jsx는 props가 변경됐다고 인식하여 다시 렌더링이 되는 것이다.

<br>

## 3.3 useCallback 적용

이를 해결하기 위해 아래 함수를 메모리 공간에 저장해놓고, 특정 조건이 아닌 경우엔 변경되지 않도록 하면된다.

```jsx
const onInitButtonClickHandler = () => {
  initCount();
};
```

<br>

useCallback hook의 적용하면 아래와 같다.

```jsx
import React, { useState, useCallback } from "react";
```

```jsx
// 변경 전
// App.jsx
const initCount = () => {
  setCount(0);
};

// 변경 후 → 최초의 주소값만 전달받음
const initCount = useCallback(() => {
  setCount(0);
}, []);
```

![](/assets/images/2024/2024-01-29-09-38-56.png)

<br>

## 3.4 useCallback와 dependency array

count를 초기화 할 때, 콘솔을 찍어보자

```jsx
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, []);
```

![](/assets/images/2024/2024-01-29-09-52-08.png)

카운터를 증가/감소시키고 초기화했을 때 항상 0에서 0으로 변경되었다는 메세지가 출력된다. 왜 그럴까?

useCallback이 count가 0일 때의 시점을 기준(스냅샷)으로 메모리에 함수를 저장했기 때문이다. 따라서 dependency array가 필요하다.

<br>

기존 코드의 dependency array에 count를 넣으면, count가 변경 될 때 마다 새롭게 함수를 할당하게 되므로 정상적으로 출력된다. (count가 변경될 때 만큼은 메모리에 다시 저장)

즉, dependency array가 변경되지 않는 이상 초기화되지 않는다.

```jsx
// count를 초기화해주는 함수
const initCount = useCallback(() => {
  console.log(`[COUNT 변경] ${count}에서 0으로 변경되었습니다.`);
  setCount(0);
}, [count]);
```

![](/assets/images/2024/2024-01-29-09-54-25.png)

<br><br>

# 4. useMemo 개요

## 4.1 useMemo 개념

memo는 memoization(기억)을 뜻한다.

동일한 값을 반환하는 함수를 계속 호출해야 하면 필요없는 렌더링을 하는데, 맨 처음 해당 값을 반환할 때 그 값을 메모리에 저장한다.

따라서 이미 저장한 값을 단순히 꺼내와서 쓸 수 있는것이다.(캐싱)

<br>

## 4.2 useMemo 사용방법

```jsx
// as-is
const value = 반환할_함수();

// to-be
const value = useMemo(() => {
  return 반환할_함수();
}, [dependencyArray]);
```

dependency Array의 값이 변경 될 때만 `반환할_함수()`(값)가 호출된다.

그 외의 경우에는 memoization 해놨던 값을 가져오기만 한다.

<br>

## 4.3 useMemo 적용

HeavyComponent 안에서는 `const value = heavyWork()` 를 통해서 value값을 세팅해주고 있다.

만약 heavyWork가 엄청나게 무거운 작업이라면 다른 state가 바뀔 때 마다 계속해서 호출이 되기때문에 `useMemo()`로 감싸줘야한다.

```jsx
import "./App.css";
import HeavyComponent from "./HeavyComponent";

function App() {
  return <HeavyComponent />;
}

export default App;
```

<br>

```jsx
// components/HeavyComponent.jsx
import React, { useState, useMemo } from "react";

function HeavyComponent() {
  const [count, setCount] = useState(0);

  const heavyWork = () => {
    for (let i = 0; i < 1000000000; i++) {}
    return 100;
  };

  // CASE 1 : useMemo를 사용하지 않았을 때
  const value = heavyWork();

  // CASE 2 : useMemo를 사용했을 때
  // const value = useMemo(() => heavyWork(), []);

  return (
    <>
      <p>{value}을 가져오는 엄청 무거운 작업을 하는 컴포넌트</p>
      <button
        onClick={() => {
          setCount(count + 1);
        }}
      >
        증가
      </button>
      <br />
      {count}
    </>
  );
}

export default HeavyComponent;
```

![](/assets/images/2024/2024-01-29-10-27-47.png)

확실히 useMemo를 사용했을 때랑 사용하지 않았을 때 버튼 눌리는 속도가 다르다.

<br>

## 4.4 useMemo와 dependency array

```jsx
import React, { useEffect, useState } from "react";

function ObjectComponent() {
  const [isAlive, setIsAlive] = useState(true);
  const [uselessCount, setUselessCount] = useState(0);

  const me = {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };

  useEffect(() => {
    console.log("생존여부가 바뀔 때만 호출해주세요!");
  }, [me]);

  return (
    <>
      <div>
        내 이름은 {me.name}이구, 나이는 {me.age}야!
      </div>
      <br />
      <div>
        <button
          onClick={() => {
            setIsAlive(!isAlive);
          }}
        >
          누르면 살았다가 죽었다가 해요
        </button>
        <br />
        생존여부 : {me.isAlive}
      </div>
      <hr />
      필요없는 숫자 영역
      <br />
      {uselessCount}
      <br />
      <button
        onClick={() => {
          setUselessCount(uselessCount + 1);
        }}
      >
        누르면 숫자가 올라가요
      </button>
    </>
  );
}

export default ObjectComponent;
```

![](/assets/images/2024/2024-01-29-10-34-53.png)

useEffect hook을 이용해서 me의 정보가 바뀌었을 때만 발동되게끔 dependency array를 넣어놨는데, count를 증가하는 button을 눌러보면 계속 log가 찍히는 것을 볼 수가 있다. 왜 그럴까?

<br>

> 불변성과 관련이 깊다.

위 예제에서 버튼이 선택돼서 uselessCount state가 바뀌게 되면

1. 리렌더링이 된다.
2. 컴포넌트 함수가 새로 호출된다.
3. me 객체도 다시 할당된다.(이때, 다른 메모리 주소값을 할당받는다.)
4. useEffect의 dependency array에 의해 me 객체가바뀌었는지 확인해봐야 하는데 주소가 다르므로 리액트 입장에서는 me가 바뀌었구나 인식하고 useEffect 내부 로직이 호출된다.

<br>

이런 상황을 해결하기 위해서 또한 useMemo를 활용할 수 있다.

```jsx
const me = useMemo(() => {
  return {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };
}, [isAlive]);
```

useMemo()만 이렇게 써주면, uselessCount가 아무리 증가돼도 영향이 없게 된다.
<br>

## 4.5 주의사항

useMemo를 남발하게 되면 별도의 메모리 확보를 너무나 많이 하게 되기 때문에 오히려 성능이 악화될 수 있다.

<br>
