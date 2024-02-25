---
title: "[React P.J, ForYou] context api 적용하기 #6"
categories: [Project]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> props drilling으로 불편하게 관리하던 state를 context api 로 리팩토링 해보자!

① src/context/LetterContext.jsx 생성

```js
import { createContext, useState } from "react"; // (1) import
import fakeData from "fakeData.json"; // // 옮겨진 데이터

export const LetterContext = createContext(null); // (2) Context 생성

// (3) Provider 생성 :
// children props를 사용해서 LetterContextProvider로 감싸지는 모든 자식 컴포넌트들이 value 값을 공유할 수 있도록 한다.
function LetterContextProvider({ children }) {
  // (4) props drilling으로 공유되는 useState를 옮겨주자
  const [letters, setLetters] = useState(fakeData);

  return (
    <LetterContext.Provider value={{ letters, setLetters }}>
      {children}
    </LetterContext.Provider>
  );
}

export default LetterContextProvider;
```

<br>

② 그 후, index.jsx에서 LetterContextProvider로 컴포넌트를 감싸주자<br>
이제 App 컴포넌트 안에 있는 모든 컴포넌트들이 letters, setLetters를 사용할 수 있다!

```js
// index.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import GlobalStyle from "styles/GlobalStyle";
import LetterContextProvider from "context/LetterContext"; // import

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <LetterContextProvider>
    <App />
    <GlobalStyle />
  </LetterContextProvider>
);
```

<br>

③ 아래와 같은 식으로 props로 letters, setLetters를 내려주는 코드를 삭제해주면 된다.

```js
// 삭제 전
<Route path="/" element={<Home letters={letters} setLetters={setLetters} />} />
<Route path="/detail/:id" element={<Detail letters={letters} setLetters={setLetters} />} />

// 삭제 후
<Route path="/" element={<Home />} />
<Route path="/detail/:id" element={<Detail />} />
```

```js
// 삭제 전
function Home({ letters, setLetters })

// 삭제 후
function Home()
```

<br>

④ 삭제 후, props를 직접적으로 내려받는 컴포넌트는 아래와 같이 context를 적용하면 된다.

```js
// 적용 전
function AddForm({ activeTab, setLetters })

// 적용 후
function AddForm({ activeTab }) {
    const { setLetters } = useContext(LetterContext);
}


// 적용 전
function LetterList({ activeTab, letters })

// 적용 후
function LetterList({ activeTab }) {
  const { letters } = useContext(LetterContext);
}


// 적용 전
function Detail({ letters, setLetters })

// 적용 후
function Detail() {
  const { letters, setLetters } = useContext(LetterContext);
}
```

<br><br>

> 마찬가지로 activeTab state로 context api를 적용해줬다!

```js
import { createContext, useState } from "react";

export const ActiveTabContext = createContext(null);

function ActiveTabContextProvider({ children }) {
  const [activeTab, setActiveTab] = useState("토토로");
  return (
    <ActiveTabContext.Provider value={{ activeTab, setActiveTab }}>
      {children}
    </ActiveTabContext.Provider>
  );
}

export default ActiveTabContextProvider;
```

<br>

위에서 한 것처럼 index.jsx에서 LetterContextProvider로 컴포넌트를 감싸주자!

```js
// index.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import GlobalStyle from "styles/GlobalStyle";
import LetterContextProvider from "context/LetterContext";
import ActiveTabContext from "context/ActiveTabContext";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <ActiveTabContext>
    <LetterContextProvider>
      <App />
      <GlobalStyle />
    </LetterContextProvider>
  </ActiveTabContext>
);
```

<br>

마찬가지로 context api를 적용하면 된다.

```js
// 적용 전
function Tabs({ activeTab, setActiveTab }) {}

// 적용 후
function Tabs() {
  const { activeTab, setActiveTab } = useContext(ActiveTabContext);
}
``;
```

<br>
