---
title: "[React] React Hooks - useContext"
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

# 1. useContext 개요

## 1.1 react context 등장 배경

일반적으로 부모컴포넌트 → 자식 컴포넌트로 데이터를 전달해 줄 때 props를 사용하였다. 하지만, 이 방식으로 데이터를 전달했을 때 prop drilling 현상이 일어난다는 문제점이 있다.

> prop drilling의 문제점

1. 깊이가 너무 깊어지면 prop이 어떤 컴포넌트로부터 왔는지 파악이 어려워진다.
2. 어떤 컴포넌트에서 오류가 발생할 경우 추적이 힘들어지니 대처가 늦을 수 밖에 없다.<br><br>
   ![](/assets/images/2024/2024-01-27-14-51-47.png)
   자료 : https://www.copycat.dev/blog/react-context/

따라서 이러한 문제를 해결하기 위해 react context API가 등장하게 되었다. useContext hook을 통해 쉽게 전역 데이터를 관리할 수 있다.

<br>

## 1.2 context API 필수 개념

> useContext Hook은 Context 객체를 받아 해당 Context의 현재 값을 반환한다. 이 Hook을 사용하면 Context의 Provider에서 제공한 값을 자식 컴포넌트에서 바로 사용할 수 있다.

- `createContext` : context 생성
- `Consumer` : context 변화 감지
- `Provider` : context 전달(to 하위 컴포넌트)

<br>

## 1.3 범위 상태

> context 는 문맥, 맥락이라는 의미이다.

- 아래 사진에서 A라는 컴포넌트 하위에 있는 컴포넌트들은 동일한 맥락에서 대화를 하고 있다.
- useContext를 root 경로에 두었을 때, 비로소 전역상태라고 할 수 있다.
- 예시를 들면, 각 나라마다 다른 언어를 사용한다고 했을 때 나라의 꼭대기에 context를 잡아주면 하위에 있는 모든 국민들은 동일한 언어(데이터)를 사용할 수 있는 것이다.

![](/assets/images/2024/2024-07-09-01-04-43.png)

<br><br>

# 2. useContext 구현하기

Context를 사용해서 isDark라는 데이터를 모든 하위 컴포넌트들에게 props를 사용하지 않고 공유해보도록하자.

## 2.1 Context 생성

- children?
  - children은 해당 컴포넌트의 여는 태그와 닫는 태그 사이에 포함된 모든 요소나 컴포넌트를 의미한다.
  - 아래 코드에서 children은 ThemeContextProvider 컴포넌트로 감싸진 모든 하위 컴포넌트를 의미한다.
  - children props를 사용해서 ThemeContextProvider로 감싸지는 모든 자식 컴포넌트들이 value 값을 공유할 수 있다.

{% raw %}

```jsx
// src/context/ThemeContext.jsx

// (1) import
import { createContext, useState } from "react";

// (2) Context 생성
export const ThemeContext = createContext(null);

// (3) Provider 생성
export const ThemeContextProvider = ({ children }) => {
  // (4) props drilling으로 공유되는 useState를 옮겨주자
  const [isDark, setIsDark] = useState(false);

  return (
    <ThemeContext.Provider value={{ isDark, setIsDark }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

{% endraw %}

<br>

## 2.2 Context 제공 (Provider)

```jsx
// index.jsx
import ReactDOM from "react-dom/client";
import App from "./App";
import { ThemeContextProvider } from "./context/ThemeContext"; // (1) 만든 Context import

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  // (2) 만든 Provider 로 감싸기
  // (3) value엔 전달하고자하는 데이터 넣기
  <ThemeContextProvider>
    <App />
  </ThemeContextProvider>
);
```

<br>

## 2.3 useContext를 통한 값 사용

```jsx
// components/pages/MainPage.jsx
import { ThemeContext } from "context/ThemeContext";
import { useContext } from "react";
import styled, { css } from "styled-components";

const MainPage = () => {
  // useContext Hook으로 Context로 전달한 정보 받아오기
  const { isDark, setIsDark } = useContext(ThemeContext);

  const toggleTheme = () => {
    setIsDark((prev) => !prev);
  };

  return (
    <MainLayout $isDark={isDark}>
      <p>안녕</p>
      <p>Hello</p>
      <button onClick={toggleTheme}>
        {isDark ? "라이트 모드" : "다크모드"}
      </button>
    </MainLayout>
  );
};

export default MainPage;

// const MainLayout = styled.main`
//   height: 100vh;
//   background-color: ${(props) => (props.isDark ? '#000000' : '#ffffff')};
// `;
const MainLayout = styled.main`
  height: 100vh;
  ${(props) =>
    props.$isDark
      ? css`
          background-color: black;
        `
      : css`
          background-color: white;
        `}
`;
```

![](/assets/images/2024/2024-07-09-02-44-54.png)

<br>

# 3. 커스텀 훅으로 Context 사용하기

## 3.1 커스텀 훅 만들기

> 여러 컴포넌트에서 Context 값을 사용하려면 매번 useContext를 호출해야한다. 커스텀 훅을 만들면 Context를 좀 더 간편하게 사용할 수 있다.

아래는 AuthContext를 사용하여 인증 상태를 관리하는 커스텀 훅 useAuth를 만드는 예시이다.

{% raw %}

```jsx
// src/context/AuthContext.jsx
import { getProfile } from "api/auth";
import { createContext, useContext, useEffect, useState } from "react";

// 1. AuthContext 생성
const AuthContext = createContext();

// 2. AuthContext Provider
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const isSignIn = !!user; // 로그인 상태 확인

  useEffect(() => {
    const fetchUser = async () => {
      try {
        const response = await getProfile();
        setUser(response.data.member);
      } catch (error) {
        setUser(null);
      } finally {
        setLoading(false);
      }
    };
    fetchUser();
  }, []);

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <AuthContext.Provider value={{ user, setUser, isSignIn }}>
      {children}
    </AuthContext.Provider>
  );
};

// 3. useAuth 커스텀 훅 생성
export const useAuth = () => useContext(AuthContext);
```

{% endraw %}

<br>

## 3.2 커스텀 훅 사용하기

> 생성한 useAuth 훅을 호출하여 생성한 `user`, `setUser`, `isSignIn` 을 사용할 수 있다.

`const { user, login, logout, isSignIn } = useAuth();` 와 같이 사용할 수 있다.

{% raw %}

```jsx
import { useAuth } from "context/AuthContext";
import { Navigate, Outlet, useLocation } from "react-router-dom";

const ProtectedRoute = () => {
  const { isSignIn } = useAuth(); // 커스텀 훅 사용하기
  const location = useLocation();

  if (!isSignIn) {
    return <Navigate to="/sign-in" state={{ from: location }} replace />;
  }

  return <Outlet />;
};

export default ProtectedRoute;
```

{% endraw %}

<br><br>

# 3. 주의사항

> 재사용성

Context를 사용하면 컴포넌트를 재사용하기 어려워질 수 있다. 필요할 때만 사용하는 것이 좋다.

<br>

> 렌더링 문제

- useContext를 사용할 때, Provider에서 제공한 value가 달라진다면 useContext를 사용하고 있는 모든 컴포넌트가 리렌더링 된다.
- 즉, value 부분을 항상 신경써줘야 한다. → 따라서 메모이제이션이 필요하다.

<br><br>

# 3. 참조

- https://www.youtube.com/watch?v=JQ_lksQFgNw

<br>
