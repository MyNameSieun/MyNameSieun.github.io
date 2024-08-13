---
title: "[React] React Hooks - useEffect"
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

# 1. useEffect 개요

## 1.1 useEffect 개념

① useEffect는 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

② 어떤 컴포넌트가 화면에 첫 렌더링이 됐을 때(mount), 다시 렌더링 될 때(Update), 화면에서 사라졌을 때(unmount) 특정 작업을 처리하고 싶다면 useEffect를 사용한다.

③ `import React, { useEffect } from "react";` 로 import 해서 사용한다.

④ ⭐ useEffect는 컴포넌트의 렌더링(혹은 리렌더링)이 모두 끝난 후에 실행된다.

<br>

## 1.2 useEffect 원리

useEffect 훅은 기본적으로 콜백함수를 받는다.<br>
콜백함수 내부에 원하는 작업을 작성해주면 된다.

```js
useEffect(() => {실행하고 싶은 함수});
```

<br>

브라우저에서 컴포넌트가 화면에 렌더링될 때 useEffect 안에 있는 console.log가 실행된다.

```jsx
import { useEffect } from "react";

const App = () => {
  useEffect(() => {
    // 이 부분이 실행된다.
    console.log("hello useEffect");
  });

  return <div>Home</div>;
};

export default App;
```

![](/assets/images/2024/2024-01-31-16-12-48.png)

<br>

## 1.3 useEffect와 리렌더링

useEffect는 컴포넌트가 렌더링 될 때에 특정 작업을 수행한다는 특징 때문에 아래와 같은 문제가 발생한다.

아래 코드를 콘솔에서 확인해보면 브라우저에 input에 어떤 값을 입력했을 때 useEffect가 계속 실행되는 것을 볼 수 있다.

```jsx
import { useEffect, useState } from "react";

const App = () => {
  const [value, setValue] = useState("");

  useEffect(() => {
    console.log("안녕!");
  });

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={(e) => {
          setValue(e.target.value);
        }}
      />
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-26-14-48-52.png)

<br>

왜 input에 값을 입력한 것 뿐인데, useEffect가 계속 실행되는 것일까?

1. input에 값을 입력한다.
2. value, 즉 state가 변경된다.
3. state가 변경되었기 때문에, App 컴포넌트가 리렌더링 된다.
4. 리렌더링이 되었기 때문에 useEffect가 다시 실행된다.
5. 1번 → 4번 과정이 계속 순환환다.

따라서 input을 입력할 때마다 렌더링이 다시 되고 있어 계속 콘솔이 찍히고 있는 것이다.

<br>

맨 처음에 로딩이 됐을 때만 useEffect를 실행할 수 있는 방법을 아래에서 알아보자!

<br><br>

# 2. 의존성 배열

> 의존성 배열(dependency array)이란?

의존성 배열을 통해 함수의 실행 조건을 제어할 수 있다.

즉, 컴포넌트가 렌더링 될 때와 의존성 배열의 값이
변경될 때 useEffect를 실행할 수 있는 것이다.

```jsx
// useEffect의 두번째 인자가 의존성 배열이 들어가는 곳
useEffect(() => {
  // 실행하고 싶은 함수
}, [의존성배열]);
```

<br>

> 의존성 배열에 빈 배열 [ ] 을 넣은 경우

useEffect 에서 함수를 1번만 실행시키고자 할때는 의존성 배열을 빈 배열로 두면 된다.

⭐ 의존성배열이 비어있는 경우 최초 렌더링 할 때만 실행되기 때문이다.(DB 통신시 유용)

```jsx
import  useEffect, useState } from "react";

const App = () => {
  const [value, setValue] = useState("");

  useEffect(() => {
    console.log("hello useEffect");
  }, []); // 비어있는 의존성 배열

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={(event) => {
          setValue(event.target.value);
        }}
      />
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-26-14-53-52.png)

input에 어떤 값을 입력하더라도, 처음에 실행된 hello useEffect외에는 더 이상 실행이 되지 않는다.

<br>

> 의존성 배열에 값이 있는 경우

value는 state이고 우리가 input을 입력할 때마다 그 값이 변하게 되니 useEffect도 계속 실행된다.

```jsx
import { useEffect, useState } from "react";

const App = () => {
  const [value, setValue] = useState("");

  useEffect(() => {
    console.log("hello useEffect");
  }, [value]); // value를 넣음

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={(event) => {
          setValue(event.target.value);
        }}
      />
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-31-16-41-00.png)

<br><br>

# 3. clean up

## 3.1 clean up 개념

컴포넌트가 사라졌을 때 무언가를 실행하는 과정을 클린 업(clean up) 이라고 한다.

useEffect가 실행될 때 반환된 함수로, 주로 특정 작업이 더 이상 필요하지 않을 때 자원을 정리하거나 해제하는 용도로 사용한다.

<br>

## 3.2 clean up 사용 방법

useEffect 안에서 return 을 해주고 이 부분에 실행되길 원하는 함수를 넣으면 된다. (그냥 console.log와 같다.)

```jsx
import { useEffect } from "react";

const App = () => {
  useEffect(() => {
    // 화면에 컴포넌트가 나타났을(mount) 때 실행하고자 하는 함수 넣기

    return () => {
      // 화면에서 컴포넌트가 사라졌을(unmount) 때 실행하고자 하는 함수 넣기
    };
  }, []);

  return <div>hello react!</div>;
};

export default App;
```

<br>

## 3.3 clean up 사용하기

타이머를 설정하고 해당 컴포넌트가 언마운트될 때 타이머를 제거하는 상황을 생각해보자

```jsx
// src/App.js
import { useEffect, useState } from "react";
import Timer from "./components/Timer";

const App = () => {
  const [showTimer, setShowTimer] = useState(false);

  return (
    <div>
      {showTimer && <Timer />}
      <button onClick={() => setShowTimer(!showTimer)}>Toggle Timer</button>
    </div>
  );
};

export default App;
```

```js
// components/Timer.jsx
import { useEffect } from "react";

const Timer = (props) => {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log("타이머 실행중..");
    }, 1000);
  }, []);
  return <div>타이머 시작!</div>;
};

export default Timer;
```

![](/assets/images/2024/2024-01-31-16-57-48.png)
![](/assets/images/2024/2024-01-31-16-57-55.png)

![](/assets/images/2024/2024-01-31-16-58-08.png)

토글 버튼이 해제됐는데도 타이머가 실행중인 것을 볼 수 있다.

<br>

타이머가 컴포넌트가 언마운트 될 때 타이머가 멈추도록 코드를 변경해보자.

useEffect의 return값으로 정리 작업을 할 함수를 주면 된다.

```jsx
// components/Timer.jsx
import React, { useEffect } from "react";

const Timer = (props) => {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log("타이머 실행중..");
    }, 1000);

    // 아래 코드 추가
    return () => {
      clearInterval(timer);
      console.log("타이머가 종료되었습니다.");
    };
  }, []);
  return <div>타이머 시작!</div>;
};

export default Timer;
```

![](/assets/images/2024/2024-01-31-17-02-11.png)

<br>
