---
title: "[React] React Router Dom"
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

- 리액트 라우터(React Router)는 리액트 애플리케이션에서 페이지 이동을 구현할 수 있는 라이브러리이다.
- 이를 통해 SPA(Single Page Application) 환경에서 URL에 따라 다른 컴포넌트를 렌더링할 수 있다.

<br>

## 1.2 react-router-dom 설치하기

vscode 터미널에서 아래 코드를 입력해서 패키지를 설치하자.

```bash
yarn add react-router-dom
```

<br><br>

# 2. react-router-dom 사용하기

## 2.1 페이지 컴포넌트 생성

여러개의 페이지를 이동하기 위해 가상의 페이지를 만들어보자.

src/pages에 `Home`, `About`, `Contact`, `Works` 총 4개의 컴포넌트를 만들어줬다.

특별한 내용이 없이 단순히 컴포넌트의 이름만 JSX에 넣어주었다.

<br>

> ② Router.jsx 생성 및 route 설정 코드 작성⭐

- BrowserRouter를 Router로 감싸는 이유는, SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어준다.
- path에다가 사용하고싶은 주소를 넣어주고, element에는 해당 주소로 이동했을 때 보여주고자 하는 컴포넌트를 넣어준다.

```jsx
// src > shared > Router.jsx

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
        {/* 사용자가 잘못된 경로로 이동했을 때 기본적으로 (/)로 리다이렉션 할 수 있다. */}
        <Route path="*" element={<Navigate replace to="/" />} />; // //
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

## 2.1 App.jsx에 `Router.jsx` import 해주기

생성한 Router 컴포넌트를 아래 코드와 같이 App.jsx에 import 해준다.

```jsx
import Router from "./shared/Router";

function App() {
  return <Router />;
}

export default App;
```

<br>

## 2.3 페이지 이동 테스트

페이지가 잘 이동하는지 확인해보자.

```
localhost:3000/home
localhost:3000/about;
localhost:3000/contact
localhost:3000/works
```

![](/assets/images/2024/2024-01-30-11-09-13.png)

잘 이동한다!

<br>

# 3. 동적 라우팅(Dynamic Route)

## 3.1 동적 라우팅 개요

- 동적 라우트에서는 경로에 따라 컴포넌트를 동적으로 할당하고, 경로의 일부를 매개변수로 사용하여 컴포넌트를 렌더링하는 것이 가능하다.
- 즉, path에 유동적인 값을 넣어서 특정 페이지로 이동하게끔 할 수 있는 것이다.
- 동적 라우트를 사용하면 /users/1, /users/2, /users/3과 같은 다양한 사용자 프로필을 동일한 컴포넌트로 처리할 수 있다.

```jsx
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* 비효율적 */}
        <Route path="/" element={<Home />} />
        <Route path="/info/1" element={<Info />} />
        <Route path="/info/2" element={<Info />} />
        <Route path="/info/3" element={<Info />} />
      </Routes>
    </BrowserRouter>
  );
};
```

위 코드를 react-router-dom에서 지원하는 Dynamic Routes 기능을 이용해서 간결하게 동적으로 변하는 페이지를 처리해보자.

<br>

## 3.2 params 설정하기

> `useParams` 이라는 훅을 사용하여 Route의 element에 넣어준 컴포넌트에서 parms 값을 가져올 수 있다.

- useParams 은 path의 있는 id 값을 조회할 수 있게 해주는 훅이다.'
- useParam을 이용하면 같은 컴포넌트를 렌더링하더라도 각각의 고유한 `id` 값을 조회할 수 있다.

```jsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/info/:id" element={<Info />} />
  </Routes>
</BrowserRouter>
```

<br>

## 3.3 params 받아오기

```jsx
import { useParams } from "react-router-dom";

const Info = () => {
  const { id } = useParams(); // URL 파라미터에서 id값을 추출

  return <div>Info ID: {id}</div>;
};

export default Info;
```

![](/assets/images/2024/2024-07-10-21-07-47.png)

<br>

## 3.4 응용 - id에 따른 조건부 렌더링

> info.json

```json
[
  {
    "id": 1,
    "title": "내가 제일 좋아하는 아이스크림은?",
    "content": "부라보콘 피스타치오 맛!!!🍦🍦"
  },
  {
    "id": 2,
    "title": "내가 제일 좋아하는 음식은?",
    "content": "돈가스!! 💰💰💲"
  }
]
```

```jsx
import infoData from "info.json";
import { useParams } from "react-router-dom";

const Home = () => {
  const { id } = useParams();

  return (
    <div>
      {infoData
        .filter((info) => info.id === parseInt(id))
        .map((info) => (
          <div key={info.id}>
            <p>{info.title}</p>
            <p>{info.content}</p>
          </div>
        ))}
    </div>
  );
};

export default Home;
```

![](/assets/images/2024/2024-07-10-21-58-03.png)

<br><br>

# 4. Link 이동하기

## 4.1 useNavigate 사용하기

> `useNavigate` 훅을 사용하면 코드 로직을 통해 페이지 이동을 제어할 수 있다.

즉, 어떤 버튼이나 컴포넌트를 눌렀을 때 페이지로 이동하게 만들기 위해 사용하는 훅이다.

```jsx
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();

  return (
    <>
      <button onClick={() => navigate("/info")}>Info 페이지로 이동</button>
      <button onClick={() => navigate(1)}>앞으로 가기</button>
      <button onClick={() => navigate(-1)}>뒤로 가기</button>
    </>
  );
};

export default Home;
```

> 사용 사례

- 게시글을 삭제하고 홈 화면으로 넘어가기
- 게시글 작성시 로그인이 되어있지 않으면 로그인 화면으로 이동하기
- 폼 제출 후 유효성 검사한 뒤 홈으로 넘어가기
- 앞으로 가기, 뒤로 가기

<br>

## 4.2 Link 사용하기

> `Link` 는 html 태그중에 `<a>` 태그의 기능을 대체하는 API이다.

- `<a>` 태그를 사용하면, 브라우저가 새로고침 되면 모든 컴포넌트가 다시 렌더링되야 하고, 또한 우리가 리덕스나 useState를 통해 메모리상에 구축해놓은 모든 상태값이 초기화 되서 성능이 저하될 수 있다.
- 따라서, `Link` 를 사용하여 SPA 환경에서 페이지 리로드 없이 라우트를 변경해야한다.

```jsx
import { Link } from "react-router-dom";

const Home = () => {
  return (
    <nav>
      <Link to="/info">Info 페이지로 이동</Link>
    </nav>
  );
};

export default Home;
```

> 사용 사례

- Navbar
- 게시글 리스트의 아이템
- 바로가기 링크
- 사용자가 직접 클릭하여 페이지를 이동할 수 있는 명시적인 경로가 필요할 때
- 시맨틱 웹 구조가 필요할 때

<br><br>

# 5. useLocation를 활용하여 페이지 정보 얻기

> `react-router-dom`을 사용하면, 현재 위치하고 있는 페이지의 여러가지 정보를 추가적으로 얻을 수 있다.

이 정보들을 이용해서 페이지 안에서 조건부 렌더링에 사용하는 등, 여러가지 용도로 활용할 수 있다.

```jsx
import { useLocation } from "react-router-dom";

const Home = () => {
  const location = useLocation();
  console.log("location: ", location);

  return <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>;
};

export default Home;
```

![](/assets/images/2024/2024-07-10-21-27-00.png)

<br>
