---
title: "[React] throttling & debouncing"
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

# 1. throttling & debouncing 개요

짧은 시간 간격으로 연속해서 이벤트가 발생했을 때 과도한 이벤트 핸들러 호출을 방지하는 기법을 말한다.

<br>

## 1.1 throttling 개념

짧은 시간 간격으로 연속해서 발생한 이벤트들을 일정시간 단위(delay)로 그룹화하여 처음 또는 마지막 이벤트 핸들러만 호출되도록 하는 것을 말한다. (예: 무한스크롤)

즉, 입력 주기를 방해하지 않고, 일정 시간 동안의 입력을 모와서, 한 번씩 출력을 제한한다.

![](/assets/images/2024/2024-02-19-11-34-31.png)

<br>

## 1.2 debouncing 개념

짧은 시간 간격으로 연속해서 이벤트가 발생하면 이벤트 핸들러를 호출하지 않다가 마지막 이벤트로부터 일정 시간(delay)이 경과한 후에 한 번만 호출하도록 하는 것을 말한다. (예: 입력값 실시간 검색, 화면 resize 이벤트)

즉, 입력 주기가 끝나면 출력한다.

![](/assets/images/2024/2024-02-19-11-34-56.png)

<br><br>

# 2. 실습

## 2.1 throttle & debouncing 구현

먼저 react-router-dom을 설치해주자

```
yarn add react-router-dom
```

<br>

> timer id(Throttling과 debounce을 제어하는 키)

setTimeout 함수는 타이머 ID를 반환한다.<br>
이 타이머 ID는 나중에 clearTimeout 등의 메서드를 사용하여 해당 타이머를 취소하거나 제어하는 데 사용한다.

![](/assets/images/2024/2024-02-19-12-13-25.png)

<br>

```js
// src > pages > Home.jsx
export default function Home() {
  // timer id(Throttling과 debounce을 제어하는 키)
  let timerId = null;

  // Leading Edge Throttling
  const throttle = (delay) => {
    if (timerId) {
      // timerId가 있으면 바로 함수 종료
      return;
    }
    console.log(`API 요청 실행. ${delay}ms 동안 추가 요청은 받지 않습니다.`);
    timerId = setTimeout(() => {
      console.log(`${delay}ms가 지났으므로 추가 요청을 받습니다.`);
      timerId = null;
    }, delay);
  };

  // 반복적인 이벤트 이후, delay가 지나면 function
  const debounce = (delay) => {
    if (timerId) {
      // 할당되어있는 timerId에 해당하는 타이머 제거
      clearTimeout(timerId);
    }
    timerId = setTimeout(() => {
      console.log(`마지막 요청으로부터${delay}ms 지났으므로 API 요청 실행`);
      timerId = null;
    }, delay);
  };

  return (
    <div style={{ paddingLeft: 20, paddingRight: 20 }}>
      <h1>Button 이벤트 예제</h1>
      <button onClick={() => throttle(2000)}>쓰로틀링 버튼</button>
      <button onClick={() => debounce(2000)}>디바운싱 버튼</button>
    </div>
  );
}
```

<br>

하지만, 아래와 같이 state를 설정했을 경우 throttle이 발동할 때 state가 변경되면서 리렌더링이 되므로 다시 함수가 호출되면서 `let timerId = null;`로 설정되므로 정상적으로 동작하지 않는 문제가 있다.

```js
let timerId = null;
const [state, setState] = useState(true); // state 생성

// Leading Edge Throttling
const throttle = (delay) => {
  if (timerId) {
    return;
    setState(!state); // 리렌더링 → 다시 함수 호출
  }
  console.log(`API 요청 실행. ${delay}ms 동안 추가 요청은 받지 않습니다.`);
  timerId = setTimeout(() => {
    console.log(`${delay}ms가 지났으므로 추가 요청을 받습니다.`);
    timerId = null;
  }, delay);
};
```

<br>

## 2.2 메모리 누수 문제 해결

메모리 누수(Memory Leak)란? 필요하지 않은 메모리를 계속 점유하고 있는 현상을 말한다.

<br>

아래와 같이 Router을 설정해주자.

```js
// App.jsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "./pages/Home";
import Company from "./pages/Company";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/company" element={<Company />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

```js
// src > pages > Company.jsx
import React from "react";

export default function Company() {
  return <div>Test Page</div>;
}
```

<br>

그리고 버튼 클릭시 다른 페이지로 이동할 수 있게하자

```js
import { useState } from "react";
import { useNavigate } from "react-router-dom"; // import

export default function Home() {
  let timerId = null;
  const [state, setState] = useState(true);
  const navigate = useNavigate(); // navigate 생성

  const throttle = (delay) => {
    ...
  };

  const debounce = (delay) => {
   ...
  };

  return (
    <div>
      <h1>Button 이벤트 예제</h1>
      <button onClick={() => throttle(2000)}>쓰로틀링 버튼</button>
      <button onClick={() => debounce(2000)}>디바운싱 버튼</button>

      {/* 아래 코드 추가 */}
      <div>
        <button onClick={() => navigate("/company")}>페이지 이동</button>
      </div>
    </div>
  );
}

```

<br>

만약, 쓰로틀링 버튼을 누르고 다른 페이지로 이동했을 때 어떻게될까?

콘솔창을 확인해보면 전 페이지에 요청을 하고 있는 것이 아직도 영향을 끼치는 것을 확인할 수 있다. → 메모리 누수

<br>

따라서 useEffect를 활용하여 메모리 누수 문제를 해결할 수 있다.

```js
import { useEffect, useState } from "react";
import { useNavigate } from "react-router-dom";

export default function Home() {
  let timerId = null;
  const [state, setState] = useState(true);
  const navigate = useNavigate();

  // useEffect 활용
  useEffect(() => {
    return () => {
      // unmount 시
      if (timerId) {
        clearTimeout(timerId);
      }
    };
  }, []);

  const throttle = (delay) => {
    ...
  };

  const debounce = (delay) => {
    ...
  };

  return (
    <div>
      <h1>Button 이벤트 예제</h1>
      <button onClick={() => throttle(2000)}>쓰로틀링 버튼</button>
      <button onClick={() => debounce(2000)}>디바운싱 버튼</button>
      <div>
        <button onClick={() => navigate("/company")}>페이지 이동</button>
      </div>
    </div>
  );
}
```

<br><br>

# 3. 라이브러리 Lodash

설치하기

```
yarn add lodash
```

<br>

lodash는 throttle와 debounce 함수를 제공하여 이벤트 핸들러의 호출을 제어할 수 있다.

```js
import _ from "lodash";

const debouncedFunction = _.debounce(
  (arg1, arg2) => {
    console.log("Debounced function with arguments:", arg1, arg2);
  },
  1000 // milliseconds
);

// 디바운스된 함수 호출
debouncedFunction("arg1", "arg2");
```

- 첫 번째 인자: 디바운싱할 함수
- 두 번째 인자: 디바운싱할 대기 시간 (밀리초 단위), 이 시간 동안 새로운 호출이 없을 때 함수가 호출된다.

<br>

입력값을 넣고, 디바운싱 테스트를 할 수 있는 예제를 만들어보자.

```js
import React, { useState } from "react";

function App() {
  const [searchText, setSearchText] = useState("");
  const [inputText, setInputText] = useState("");

  // 공통 함수
  const handleChange = (e) => {
    setInputText(e.target.value);
  };

  return (
    <div>
      <input
        placeholder="입력값을 넣고 디바운싱 테스트를 해보세요"
        type="text"
        onChange={handleChange}
      />

      <p>Search Text : {searchText}</p>
      <p>Input Text : {inputText}</p>
    </div>
  );
}

export default App;
```

<br>

위 코드를 베이스로 디바운싱을 적용해보자<br>
Search Text엔 디바운싱을 적용할 것이고, Input Text엔 디바운싱을 적용하지 않을 것이다.

```js
import React, { useState } from "react";
import _ from "lodash"; // import
import { useCallback } from "react";

function App() {
  const [searchText, setSearchText] = useState("");
  const [inputText, setInputText] = useState("");

  const handleSearchText = useCallback(
    _.debounce((text) => {
      setSearchText(text);
    }, 2000),
    []
  );
  const handleChange = (e) => {
    setInputText(e.target.value);
    handleSearchText(e.target.value);
  };

  return (
    <div>
      <input
        placeholder="입력값을 넣고 디바운싱 테스트를 해보세요"
        type="text"
        onChange={handleChange}
      />

      <p>Search Text : {searchText}</p>
      <p>Input Text : {inputText}</p>
    </div>
  );
}

export default App;
```

- `handleSearchText`: lodash의 debounce를 사용하여 디바운스된다. 마지막 호출 이후 2000 밀리초(2초) 동안 setSearchText의 실행을 지연시킨다.
- `handleInputChange`: 입력 필드의 변경에 대한 이벤트 핸들러이다. 입력이 변경될 때마다 새 텍스트로 handleSearchText 함수를 호출한다.
- `setSearchText`: 디바운싱 기간 후에 가장 최근의 검색 텍스트로 상태를 업데이트한다.

> useCallback의 사용

⭐ useCallback을 사용해야 디바운싱이 정상적으로 작동한다!

useCallback을 사용하여 함수를 메모이제이션하고 빈 배열([])을 의존성 배열로 전달함으로써, 해당 함수가 렌더링 중에 변하지 않도록 할 수 있다.

여기서 useCallback이 사용된 handleSearchText 함수는 \_.debounce 내부에서 클로저를 형성하게 되는데, 이 클로저 함수는 외부 함수의 변수에 계속해서 참조를 갖고있기 때문에 타이머 아이디 등을 기억할 수 있게 된다.

따라서, 매 렌더링 시에 새로운 함수가 생성되지 않고 기존의 클로저가 유지될 수 있는 것이다.

<br><br>

# 4. 참조

- https://pks2974.medium.com/throttle-%EC%99%80-debounce-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-2335a9c426ff
