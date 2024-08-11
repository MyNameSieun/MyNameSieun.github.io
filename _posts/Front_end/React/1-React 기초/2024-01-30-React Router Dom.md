---
title: "[React] React Router Dom"
categories: [React]
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
        <Route path="*" element={<Navigate replace to="/" />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

## 2.1 App.jsx에 Router.jsx import 해주기

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

## 3.4 응용(1) - params id를 활용한 상세 페이지 구현

"상세" 버튼을 클릭하면, 해당 아이템의 ID를 포함한 URL로 이동하여 Info 페이지에서 상세 정보를 볼 수 있도록 구현

```jsx
<Link to={`info/${todo.id}`}>상세</Link>
```

![](/assets/images/2024/2024-08-02-18-48-35.png)

<br>

## 3.5 응용(2) - id에 따른 조건부 렌더링

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
import { useParams } from "react-router-dom";
import infoData from "../info.json";

const InfoPage = () => {
  const { id } = useParams();
  const infoFiltered = infoData.find((info) => info.id === parseInt(id));

  if (!infoFiltered) {
    return <div>정보를 찾을 수 없습니다.</div>;
  }

  return (
    <>
      <h1>InfoPage</h1>
      <ul>
        <li>{infoFiltered.title}</li>
        <li>{infoFiltered.content}</li>
      </ul>
    </>
  );
};

export default InfoPage;
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

- `<a>` 태그를 사용하면, 브라우저가 새로고침 되면 모든 컴포넌트가 다시 렌더링되야 하고, 우리가 리덕스나 useState를 통해 메모리상에 구축해놓은 모든 상태값이 초기화 되서 성능이 저하될 수 있다.
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

<br>

## 4.3 NavLink

- NavLink는 react-router-dom 라이브러리에서 제공하는 컴포넌트로, Link와 유사하게 사용되지만, 추가적으로 현재 경로와 매칭되었을 때 특정 스타일이나 클래스를 적용할 수 있는 기능을 제공한다.
- 내비게이션 바나 메뉴에서 현재 활성화된 링크를 시각적으로 구분할 때 유용하다.

```jsx
import { NavLink } from "react-router-dom";

const Navigation = () => {
  return (
    <nav>
      <NavLink
        to="/home"
        className={({ isActive }) => (isActive ? "active" : "")}
      >
        Home
      </NavLink>
      <NavLink
        to="/about"
        className={({ isActive }) => (isActive ? "active" : "")}
      >
        About
      </NavLink>
    </nav>
  );
};
```

<br>

아래와 같이 styled-components를 사용해서도 NavLink를 사용하여 현재 경로와 일치하는 경우 특정 스타일을 적용할 수 있다.

```jsx
import { NavLink } from "react-router-dom";
import styled from "styled-components";

const Navigation = () => {
  return (
    <nav>
      <StyledNavLink to="/home">Home</StyledNavLink>
      <StyledNavLink to="/about">About</StyledNavLink>
      <StyledNavLink to="/contact">Contact</StyledNavLink>
    </nav>
  );
};

export default Navigation;

const StyledNavLink = styled(NavLink)`
  color: black;
  text-decoration: none;
  padding: 10px;

  &.active {
    color: red;
    font-weight: bold;
  }
`;
```

<br><br>

# 5. 중첩 라우트

## 5.1 중첩 라우트란?

> 중첩 라우트는 하나의 라우트 내부에 다른 라우트를 포함하는 구조이다.

- 중첩 라우트를 사용하면 복잡한 UI 구조를 보다 명확하고 체계적으로 구성할 수 있다.
- 예를 들어, 대시보드에 여러 섹션이 있는 경우, 각 섹션에 해당하는 라우트를 대시보드 라우트 내에 중첩하여 정의할 수 있다.

```jsx
/* dashboard/profile 및 /dashboard/settings는 중첩 라우트. */
<Route path="/dashboard" element={<DashboardLayout />}>
  <Route path="profile" element={<Profile />} />
  <Route path="settings" element={<Settings />} />
</Route>
```

<br>

> 사용 예시

- index 속성은 부모 경로(/)에 자동으로 렌더링되는 기본 하위 페이지를 설정한다. 이를 통해, 부모 경로에 접근했을 때 보여줄 기본 페이지를 지정할 수 있다.
- 아래 코드에서 /dashboard 경로에 접근하면 Dashboard가 렌더링되고, 그 내부에 기본적으로 Overview가 표시된다.

```jsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Layout />}>
      <Route index element={<Home />} /> {/* 루트 경로에서 기본 페이지 */}
      <Route path="dashboard" element={<Dashboard />}>
        <Route index element={<Overview />} /> {/* 기본 대시보드 페이지 */}
        <Route path="profile" element={<Profile />} />
        <Route path="settings" element={<Settings />} />
      </Route>
      <Route path="about" element={<About />} />
    </Route>

    {/* 정의되지 않은 경로는 홈 페이지로 리다이렉트 */}
    <Route path="*" element={<Navigate replace to="/" />} />
  </Routes>
</BrowserRouter>
```

> 사용 사례

- 검색 필터링: 사용자가 원하는 카테고리나 가격대에 맞는 제품을 필터링할 때
- 페이지네이션: 페이지 번호를 쿼리 문자열로 관리하여 페이지를 전환할 때
- 정렬 옵션: 정렬 기준을 URL에 쿼리 문자열로 추가하여 사용자가 선택한 정렬 옵션을 유지할 때

<br>

## 5.2 Outlet 컴포넌트

> - 중첩된 라우트의 자식 컴포넌트를 렌더링하는 데 사용된다.

공통 레이아웃을 구현할 때 유용하다.

```jsx
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<HomePage />} />
          <Route path="info" element={<InfoPage />} />
          <Route path="info/:id" element={<InfoDetailPage />} />
        </Route>

        <Route path="*" element={<Navigate replace to="/" />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

```jsx
// src/components/layouts/Layout.jsx
import { Outlet } from "react-router-dom";
import styled from "styled-components";
import Navbar from "./Navbar";

const Layout = () => {
  return (
    <StLayout>
      <StContainer>
        <Navbar />
        <Outlet /> {/* 여기에 자식 라우트 컴포넌트가 렌더링된다. */}
      </StContainer>
    </StLayout>
  );
};

export default Layout;

const StLayout = styled.main`
  width: 100%;
  height: 100vh;
`;

const StContainer = styled.div`
  width: 80%;
  background-color: #dfdfdf;
  margin: auto;
`;
```

<br>

아래와 같이 Outlet을 통해 특정 경로에 대해 로그인 여부를 확인하고, 로그인된 사용자만 해당 페이지에 접근할 수 있도록 설정할 수 있다.

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Detail from "../pages/Detail";
import Home from "../pages/Home";
import Header from "../components/layouts/Header";
import Footer from "../components/layouts/Footer";
import AuthLayout from "./AuthLayout";
import Sample01 from "../pages/Sample01";
import Sample02 from "../pages/Sample02";

const Router = () => {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/:id" element={<Detail />} />
        <Route element={<AuthLayout />}>
          <Route path="sample01" element={<Sample01 />} />
          <Route path="sample02" element={<Sample02 />} />
        </Route>

        <Route path="*" element={<Navigate replace to="/" />} />
      </Routes>
      <Footer />
    </BrowserRouter>
  );
};

export default Router;
```

<br>

```jsx
// src/shared/AuthLayout.jsx
import { useEffect, useState } from "react";
import { Outlet, useNavigate } from "react-router-dom";

const AuthLayout = () => {
  const navigate = useNavigate();
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  useEffect(() => {
    // 로그인 여부를 체크
    if (!isLoggedIn) {
      navigate("/login");
    }
  }, [isLoggedIn]);

  return (
    <div>
      <Outlet />
    </div>
  );
};

export default AuthLayout;
```

- AuthLayout 컴포넌트는 isLoggedIn이 true일 때 해당 경로에 맞는 컴포넌트를 Outlet을 통해 렌더링한다.
  - 따라서, http://localhost:3000/sample01 에서 Sample01만,
  - http://localhost:3000/sample02 에서 Sample02만 보이게 된다.

![](/assets/images/2024/2024-08-09-09-21-40.png)

<br><br>

# 6. useSearchParams

## 6.1 useSearchParams란?

> useSearchParams는 URL의 쿼리 문자열을 읽고, 수정하는 기능을 제공하는 훅이다.

- 사용자에게 검색 필터링, 페이지네이션 등 다양한 검색 기능을 구현할 수 있다.
- 쿼리 문자열은 URL의 `?` 뒤에 오는 부분으로, 예를 들어 `?category=books&price=low`와 같은 형식이다.
- searchParams 객체는 URLSearchParams의 다양한 메서드(get, set, append, delete 등)를 지원한다.

```jsx
import { useSearchParams } from "react-router-dom";

const SearchPage = () => {
  // searchParams: 현재 URL의 쿼리 파라미터를 나타내는 객체
  // setSearchParams: 쿼리 파라미터를 업데이트하는 함수
  const [searchParams, setSearchParams] = useSearchParams();

  // 쿼리 파라미터 읽기
  const paramValue = searchParams.get("paramName"); // paramName에 해당하는 값을 가져옴

  // 쿼리 파라미터 설정
  const updateSearchParams = () => {
    setSearchParams({ paramName: "newValue" }); // paramName의 값을 newValue로 설정
  };

  return (
    <div>
      <p>Parameter Value: {paramValue}</p>
      <button onClick={updateSearchParams}>Update Parameter</button>
    </div>
  );
};

export default SearchPage;
```

![](/assets/images/2024/2024-08-11-10-15-28.png)

<br>

## 6.2 useSearchParams로 검색 기능 구현하기

```jsx
// info.json
[
  {
    id: 1,
    title: "내가 제일 좋아하는 아이스크림은?",
    content: "부라보콘 피스타치오 맛!!!🍦🍦",
  },
  {
    id: 2,
    title: "내가 제일 좋아하는 음식은?",
    content: "돈가스!! 💰💰💲",
  },
  {
    id: 3,
    title: "테스트",
    content: "A B C d e f",
  },
];
```

```jsx
// src/pages/InfoPage.jsx
import { useState, useEffect } from "react";
import { useSearchParams, Link } from "react-router-dom";
import infoData from "../info.json";
import styled from "styled-components";

const InfoPage = () => {
  const [searchParams, setSearchParams] = useSearchParams(); // 쿼리 파라미터 상태
  const [query, setQuery] = useState(searchParams.get("search") || ""); // 검색 쿼리 상태
  const [filteredData, setFilteredData] = useState(infoData); // 필터링된 데이터 상태

  // 검색 버튼 클릭 시 검색어를 URL 쿼리 파라미터로 설정
  const handleSearch = (e) => {
    e.preventDefault(); // 폼 제출 시 페이지 리로드 방지
    const trimmedQuery = query.trim(); // 검색어의 공백 제거
    setSearchParams({ search: trimmedQuery }); // 쿼리 파라미터 업데이트
  };

  // 검색어가 변경될 때 필터링된 데이터 업데이트
  useEffect(() => {
    const trimmedQuery = searchParams.get("search")?.trim().toLowerCase() || "";
    if (trimmedQuery) {
      const result = infoData.filter(
        (info) =>
          info.title.toLowerCase().includes(trimmedQuery) ||
          info.content.toLowerCase().includes(trimmedQuery)
      );
      setFilteredData(result);
    } else {
      setFilteredData(infoData); // 검색어가 공백만 있는 경우 전체 데이터 표시
    }
  }, [searchParams]); // searchParams가 변경될 때마다 호출됨

  return (
    <>
      <form onSubmit={handleSearch}>
        <section>
          <input
            type="text"
            value={query}
            onChange={(e) => setQuery(e.target.value)}
            placeholder="검색어를 입력하세요"
          />
          <button type="submit">검색하기</button>
        </section>
      </form>

      <h1>InfoPage</h1>

      {filteredData.length === 0 ? (
        <div>검색 결과가 없습니다.</div>
      ) : (
        <ul>
          {filteredData.map((info) => (
            <StInfoList key={info.id}>
              <Link to={`/info/${info.id}`}>
                <li>{info.title}</li>
                <li>{info.content}</li>
              </Link>
            </StInfoList>
          ))}
        </ul>
      )}
    </>
  );
};

export default InfoPage;

const StInfoList = styled.ul`
  background-color: #b6b6b6;
  padding: 1rem;
  cursor: pointer;
  list-style: none;
  margin-bottom: 0.5rem;

  a {
    text-decoration: none;
    color: black;
  }

  &:hover {
    background-color: #a0a0a0;
  }
`;
```

- `searchParams.get("query")`
  - URL에서 query라는 이름의 쿼리 파라미터를 가져온다.
  - 예를 들어, URL이 http://example.com/search?query=apple 이면 searchParams.get("query")는 "apple"을 반환한다.
  - 쿼리 파라미터가 없다면 null을 반환하므로 query 변수는 빈 문자열로 설정된다.

![](/assets/images/2024/2024-08-11-10-37-39.png)

<br><br>

# 7. useLocation로 페이지 정보 얻기

> `react-router-dom`을 사용하면, 현재 위치하고 있는 페이지의 여러가지 정보를 추가적으로 얻을 수 있다.

이 정보들을 이용해서 페이지 안에서 조건부 렌더링에 사용하는 등, 여러가지 용도로 활용할 수 있다.

```jsx
import { useLocation } from "react-router-dom";

const HomePage = () => {
  const location = useLocation();
  console.log("location: ", location);

  return <div>{`현재 페이지 : ${location.pathname.slice(1)}`}</div>;
};

export default HomePage;
```

![](/assets/images/2024/2024-07-10-21-27-00.png)

<br>
