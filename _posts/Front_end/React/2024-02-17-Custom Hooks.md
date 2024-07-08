---
title: "[React] Custom Hooks"
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

## 1. Custom hook 개념

- 함수형 컴포넌트에서 상태 관리와 간편한 로직 재사용을 위해 사용되는 기능이다.
- 이를 통해 여러 컴포넌트에서 동일한 로직을 반복하지 않고도 코드를 재사용할 수 있다.

<br><br>

# 2. Custom hook 사용

## 2.1 예시1 - 데이터 가져오기

> 아래 코드는 fetch로 데이터를 가져오는 로직이 반복된다. 이를 Custom hook을 사용하여 개선할 수 있다.

![](/assets/images/2024/2024-07-08-13-47-16.png)

```jsx
const Day = () => {
  const { day } = useParams();
  const [word, setWord] = useState([]);

  useEffect(() => {
    fetch(`http://localhost:3001/words?day=${day}`)
      .then((res) => res.json())
      .then((data) => setWord(data));
  }, [day]);
};
```

```jsx
const DayList = () => {
  const [days, setDays] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3001/days")
      .then((res) => res.json())
      .then((data) => setDays(data));
  }, []);
};
```

<br>

> 위 코드를 Custom Hook을 사용하여 개선해보자

- 커스텀 훅을 만들때 이름은 마음대로 작성해도 되나, 파일의 이름 앞에 use 라는 키워드를 붙여줘야 한다.
- src 폴더에 보통 hooks 라는 폴더를 생성해서 커스텀 훅들을 보관하는 식으로 많은 개발자들이 디렉토리 구조를 설계한다.

```jsx
// src/components/hooks/useFetch.jsx
const useFetch = (url) => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch(url)
      .then((res) => res.json())
      .then((data) => setData(data));
  }, [url]);

  return <div>useFetch</div>;
};

export default data;
```

```jsx
import useFetch from "../hooks/useFetch";

const DayList = () => {
  const days = useFetch("http://localhost:3001/days");
};
```

```jsx
import useFetch from "../hooks/useFetch";

const Day = () => {
  const word = useFetch(`http://localhost:3001/words?day=${day}`);
};
```

이제 API로 데이터를 가져오는 부분을 Custom hook을 통해 간단히 처리할 수 있다.!

<br>

# 2.2 예시2 - 입력 관리

아래 코드를 보면 input 갯수만큼 useState, event handler이 증가하는 것을 확인할 수 있다.

```js
// src/App.jsx

import React from "react";
import { useState } from "react";

const App = () => {
  // input의 갯수가 늘어날때마다 state와 handler가 같이 증가한다.
  const [title, setTitle] = useState("");
  const onChangeTitleHandler = (e) => {
    setTitle(e.target.value);
  };

  // input의 갯수가 늘어날때마다 state와 handler가 같이 증가한다.
  const [body, setBody] = useState("");
  const onChangeBodyHandler = (e) => {
    setBody(e.target.value);
  };

  return (
    <div>
      <input
        type="text"
        name="title"
        value={title}
        onChange={onChangeTitleHandler}
      />
      <input
        type="text"
        name="title"
        value={body}
        onChange={onChangeBodyHandler}
      />
    </div>
  );
};

export default App;
```

따라서 input의 개수가 증가하면 useState와 이벤트핸들러도 같이 증가하고 그로 인해 코드의 중복이 생기는데, 위 예시처럼 반복되는 로직이나 중복되는 코드를 커스텀 훅을 통해서 관리할 수 있다.

<br>

먼저 아래와 같이 커스텀 훅을 만들어주자

```js
// src/components/hooks/useInput.js

import { useState } from "react";

const useInput = () => {
  // state
  const [value, setValue] = useState("");

  // handler
  const handler = (e) => {
    setValue(e.target.value);
  };
  return [value, handler];
};

export default useInput;
```

<br>

만든 커스텀 훅은 아래와 같이 사용할 수 있다.

```js
// src/App.jsx

// import { useState } from "react";
import useInput from "./components/hooks/useInput"; // import

const App = () => {
  const [title, onChangeTitleHandler] = useInput();
  const [body, onChangeBodyHandler] = useInput();

  // const [title, setTitle] = useState("");
  // const onChangeTitleHandler = (e) => {
  //   setTitle(e.target.value);
  // };

  // const [body, setBody] = useState("");
  // const onChangeBodyHandler = (e) => {
  //   setBody(e.target.value);
  // };

  return (
    <div>
      <input
        type="text"
        name="title"
        value={title}
        onChange={onChangeTitleHandler}
      />
      <input
        type="text"
        name="title"
        value={body}
        onChange={onChangeBodyHandler}
      />
    </div>
  );
};

export default App;
```

<br>
