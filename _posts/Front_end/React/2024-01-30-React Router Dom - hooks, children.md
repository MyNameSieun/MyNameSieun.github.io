---
title: "[React] React Router Dom - hooks, children"
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

# 1. react-router-dom 개요

## 1.1 react-router-dom 개념

react-router-dom은 React 애플리케이션에서 라우팅을 구현하기 위한 라이브러리이다.

`react-router-dom`을 배우면 여러 페이지를 가진 웹을 만들 수 있게 된다.

<br>

## 1.2 react-router-dom 설치하기

vscode 터미널에서 아래 코드를 입력해서 패키지를 설치하자.

```bash
yarn add react-router-dom
```

<br>

## 1.3 react-router-dom 사용하기

react-router-dom을 사용하기 위한 순서는 다음과 같다.

1. 페이지 컴포넌트 생성 (단일 페이지 → 다중 페이지)
2. `Router.jsx` 생성 및 router 설정 코드 작성
3. `App.jsx`에 import 및 적용
4. 페이지 이동 테스트

<br>

> ① 페이지 컴포넌트 생성

여러개의 페이지를 이동하기 위해 가상의 페이지를 만들어보자.

src/pages에 `Home`, `About`, `Contact`, `Works` 총 4개의 컴포넌트를 만들어줬다.

특별한 내용이 없이 단순히 컴포넌트의 이름만 JSX에 넣어주었다.

<br>

> ② Router.js 생성 및 route 설정 코드 작성⭐

브라우저에 URL을 입력하고 이동했을 때 사용자가 원하는 페이지 컴포넌트로 이동하게끔 만드는 부분이다.

즉, URL 1개당 페이지 컴포넌트를 매칭해주는 것이다. 이 한개의 URL을 Route 라고 한다.

<br>

예를 들어보자

```
localhost: 3000/Home;
localhost: 3000/About;
localhost: 3000/Contact;
localhost: 3000/Works;
```

<br>

src안에 shared 라는 폴더를 생성해주고, 그 안에 `Router.jsx` 파일을 생성해준 후, 아래 코드를 작성하자.

```jsx
// (1) react-router-dom을 사용하기 위해서 아래 API들을 import
import { BrowserRouter, Route, Routes } from "react-router-dom";

// (2) Router 라는 함수를 만들고 아래와 같이 작성하자.
//BrowserRouter를 Router로 감싸는 이유는,
//SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어준다.
const Router = () => {
  return (
    <BrowserRouter>
      <Routes></Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

이제 우리가 만든 4개의 페이지 컴포넌트마다 Route를 설정해보자.

```jsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "../pages/Home";
import About from "../pages/About";
import Contact from "../pages/Contact";
import Works from "../pages/Works";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* 
            path에다가 사용하고싶은 주소를 넣어주면 된다.
            element는 해당 주소로 이동했을 때 보여주고자 하는 컴포넌트를 넣어준다.
		*/}
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

아래와 같이 작성해주면 모든 경로(\*)를 (/)로 리디렉션하게 해준다. 즉, 사용자가 잘못된 경로로 이동했을 때 기본적으로 (/)로 리다이렉션 할 수 있다.

```js
// Router.jsx
import { BrowserRouter, Route, Routes, Navigate } from "react-router-dom";

<Route path="*" element={<Navigate replace to="/" />} />;
```

```js
import { BrowserRouter, Route, Routes, Navigate } from "react-router-dom";
import Home from "../pages/Home";
import About from "../pages/About";
import Contact from "../pages/Contact";
import Works from "../pages/Works";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
        <Route path="*" element={<Navigate replace to="/" />} />;
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

> ③ App.jsx에 `Router.jsx` import 해주기

생성한 Router 컴포넌트를 아래 코드와 같이 App.jsx에 넣어준다.

```jsx
import Router from "./shared/Router";

function App() {
  return <Router />;
}

export default App;
```

<br>

> ④ 페이지 이동 테스트

페이지가 잘 이동하는지 확인해보자

![](/assets/images/2024/2024-01-30-11-09-13.png)

잘 이동한다!

<br><br>

# 2. react-router-dom Hooks

가장 많이 쓰이는 것 위주로 살펴보자. [React-router-dom 공식문서](https://reactrouter.com/docs/en/v6)

## 2.1 useNavigate

seNavigate 훅을 사용하면 라우팅을 위해 컴포넌트 외부에서 history 객체를 직접 조작할 필요가 없어진다.

사용자들이 브라우저에 path를 직접 입력해서 페이지를 이동하는 일은 거의 없을 것이다.

어떤 버튼이나 컴포넌트를 눌렀을 때 페이지로 이동하게 만들기 위해 사용하는 훅이다.

<br>

사용방법은 아래와 같다. `navigate` 를 생성하고, `navigate("보내고자 하는 url")` 을 통해 페이지를 이동 시킬 수 있다.

```jsx
// src/pages/home.js
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();

  return (
    <button
      onClick={() => {
        navigate("/works");
      }}
    >
      works로 이동
    </button>
  );
};

export default Home;
```

[useNavigate 공식 문서](https://reactrouter.com/docs/en/v6/hooks/use-navigate)

<br>

## 2.2 useLocation

`react-router-dom`을 사용하면, 현재 위치하고 있는 페이지의 여러가지 정보를 추가적으로 얻을 수 있다.

이 정보들을 이용해서 페이지 안에서 조건부 렌더링에 사용하는 등, 여러가지 용도로 활용할 수 있다.

사용 방법은 아래와 같다.

```jsx
// src/pages/works.js
import { useLocation } from "react-router-dom";

const Works = () => {
  const location = useLocation();
  console.log("location :>> ", location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
    </div>
  );
};

export default Works;
```

[useLocation 공식 문서](https://reactrouter.com/docs/en/v6/hooks/use-location)

<br><br>

# 3. API

## 3.1 Link

Link 는 html 태그중에 a 태그의 기능을 대체하는 API이다.

만약 JSX에서 a 태그를 사용해야 한다면, 반드시 Link 를 사용해서 구현해야한다.<br>
a 태그를 사용하면 페이지를 이동하면서 브라우저가 새로고침(refresh)되기 때문이다.

브라우저가 새로고침 되면 모든 컴포넌트가 다시 렌더링되야 하고, 또한 우리가 리덕스나 useState를 통해 메모리상에 구축해놓은 모든 상태값이 초기화 되서 성능이 저하될 수 있다.

사용 방법은 아래와 같다.

```jsx
import { Link, useLocation } from "react-router-dom";

const Works = () => {
  const location = useLocation();
  console.log("location :>> ", location);
  return (
    <div>
      <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>
      <Link to="/contact">contact 페이지로 이동하기</Link>
    </div>
  );
};

export default Works;
```

<br><br>

# 4. children 용도

어떤 컴포넌트들은 어떤 자식 엘리먼트가 들어올지 미리 예상할 수 없는 경우가 있다.

예를들어 웹 사이트의 header가 변하지 않는다고 한다면 하위의 컴포넌트만 바꿔서(chidren만 다르게 줌으로써) 개발할 수 있을 것이다.

즉, header만 렌더링이 안되고 하위 페이지만 렌더링이 되는 식으로 페이지 전환이 일어나는 것 처럼 구현하고 싶은 것이다.

따라서 children props를 가지고 페이지 레이아웃을 만들며 개별적으로 존재하는 Header, Footer, Page를 합성하여 개발자가 의도하는 UI를 만들어주는 Layout 컴포넌트를 만들어 보자

{% raw %}

```jsx
// src/shared/Layout.js

import React from "react";

const HeaderStyles = {
  width: "100%",
  background: "black",
  height: "50px",
  display: "flex",
  alignItems: "center",
  paddingLeft: "20px",
  color: "white",
  fontWeight: "600",
};
const FooterStyles = {
  width: "100%",
  height: "50px",
  display: "flex",
  background: "black",
  color: "white",
  alignItems: "center",
  justifyContent: "center",
  fontSize: "12px",
};

const layoutStyles = {
  display: "flex",
  flexDirection: "column",
  justifyContent: "center",
  alignItems: "center",
  minHeight: "90vh",
};

function Header() {
  return (
    <div style={{ ...HeaderStyles }}>
      <span>Sparta Coding Club - Let's learn React</span>
    </div>
  );
}

function Footer() {
  return (
    <div style={{ ...FooterStyles }}>
      <span>copyright @SCC</span>
    </div>
  );
}

function Layout({ children }) {
  return (
    <div>
      <Header />
      <div style={{ ...layoutStyles }}>{children}</div>
      <Footer />
    </div>
  );
}

export default Layout;
```

{% endraw %}

{% raw %}

```jsx
// src/shared/Router.js

import React from "react";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "../pages/Home";
import About from "../pages/About";
import Contact from "../pages/Contact";
import Works from "../pages/Works";
import Layout from "./Layout";

const Router = () => {
  return (
    <BrowserRouter>
      <Layout>
        {/* children만 변경*/}
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="about" element={<About />} />
          <Route path="contact" element={<Contact />} />
          <Route path="works" element={<Works />} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};

export default Router;
```

{% endraw %}

<br>
