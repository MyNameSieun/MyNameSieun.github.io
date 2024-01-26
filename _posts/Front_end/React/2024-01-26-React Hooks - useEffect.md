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

① seEffect는 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

② 어떤 컴포넌트가 화면에 보여졌을 때(mount) 또는 사라졌을 때(unmount) 무언가를 실행하고 싶다면 useEffect를 사용한다.

③ `import React, { useEffect } from "react";` 로 import 해서 사용한다.

<br>

## 1.2 useEffect 원리

브라우저에서 App 컴포넌트가 화면에 렌더링될 때 useEffect 안에 있는 console.log가 실행된다.

즉, useEffect는 useEffect가 속한 컴포넌트가 화면에 렌더링 될 때 실행된다.

```jsx
// src/App.js

import React, { useEffect } from "react";
import "./App.css";

const App = () => {
  useEffect(() => {
    // 이 부분이 실행된다.
    console.log("hello useEffect");
  });

  return <div>Home</div>;
};

export default App;
```

![](/assets/images/2024/2024-01-26-14-43-05.png)

<br>

## 1.2 useEffect와 리렌더링

useEffect는 useEffect가 속한 컴포넌트가 화면에 렌더링 될 때 실행된다는 특징 때문에 의도치 않은 결과를 낳을 수 있다.

아래 코드를 콘솔에서 확인해보면 브라우저에 input에 어떤 값을 입력했을 때 useEffect가 계속 실행되는 것을 볼 수 있다.

```jsx
import React, { useEffect, useState } from "react";

const App = () => {
  const [value, setValue] = useState("");

  useEffect(() => {
    console.log("hello useEffect");
  });

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

![](/assets/images/2024/2024-01-26-14-48-52.png)

<br>

왜 input에 값을 입력한 것 뿐인데, useEffect가 계속 실행되는 것일까?

1. input에 값을 입력한다.
2. value, 즉 state가 변경된다.
3. state가 변경되었기 때문에, App 컴포넌트가 리렌더링 된다.
4. 리렌더링이 되었기 때문에 useEffect가 다시 실행된다.
5. 1번 → 4번 과정이 계속 순환환다.

따라서 input을 입력할 때마다 렌더링이 다시 되고 있어 계속 콘솔이 찍히고 있는 것이다.

맨 처음에 로딩이 됐을 때만 useEffect를 실행할 수 있는 방법을 아래에서 알아보자!

<br><br>

# 2. 의존성 배열

> 의존성 배열(dependency array)이란?

의존성 배열을 통해 함수의 실행 조건을 제어할 수 있다.

즉, 배열에 값을 넣으면 그 값이 바뀔 때만 useEffect를 실행할 수 있는 것이다.

```jsx
// useEffect의 두번째 인자가 의존성 배열이 들어가는 곳
useEffect(() => {
  // 실행하고 싶은 함수
}, [의존성배열]);
```

<br>

> 의존성 배열에 빈 배열 [ ] 을 넣은 경우

input에 어떤 값을 입력하더라도, 처음에 실행된 hello useEffect외에는 더 이상 실행이 되지 않는다.

즉, useEffect 에서 함수를 1번만 실행시키고자 할때는 의존성 배열을 빈 배열로 두면 된다.

```jsx
// src/App.js

import React, { useEffect, useState } from "react";

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

<br>

콘솔에 2번 찍혀 있는 것 처럼 보인다면 index.js에서 strict mod를 주석처리해주면 된다.

```js
root.render(
  // <React.StrictMode>
  <App />
  // </React.StrictMode>
);
```

<br>

> 의존성 배열에 값이 있는 경우

value는 state이고 우리가 input을 입력할 때마다 그 값이 변하게 되니 useEffect도 계속 실행된다.

```jsx
// src/App.js

import React, { useEffect, useState } from "react";

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

<br><br>

# 3. clean up

## 3.1 clean up 개념

컴포넌트가 사라졌을 때 무언가를 실행하는 과정을 클린 업(clean up) 이라고 한다.

<br>

## 3.2 clean up 방법

useEffect 안에서 return 을 해주고 이 부분에 실행되길 원하는 함수를 넣으면 된다.

```jsx
// src/App.js

import React, { useEffect } from "react";

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

## 3.3 clean up 활용하기

속세를 벗어나는 버튼을 누르면 useNavigate에 의해서 /todos로 이동하면서 속세 컴포넌트를 떠난다.

그 후, 화면에서 속세 컴포넌트가 사라지면서 useEffect의 return 부분이 실행되는 코드이다.

```jsx
// src/SokSae.js

import React, { useEffect } from "react";
import { useNavigate } from "react-router-dom";

const 속세 = () => {
  const nav = useNavigate();

  useEffect(() => {
    return () => {
      console.log(
        "안녕히 계세요 여러분! 전 이 세상의 모든 굴레와 속박을 벗어 던지고 제 행복을 찾아 떠납니다! 여러분도 행복하세요~~!"
      );
    };
  }, []);

  return (
    <button
      onClick={() => {
        nav("/todos");
      }}
    >
      속세를 벗어나는 버튼
    </button>
  );
};

export default 속세;
```

/ 에서 /todos 잘 이동했고, 그 과정에서 clean up이 실행된 것을 확인할 수 있다.

<br>
