---
title: "[React] React Router v6.4+ 를 사용해서 인증 기반 라우팅 구현하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 0. 폴더 구조

> 폴더 구조는 다음과 같다.

```markdown
my-app:.
│
├── 📄 App.jsx
├── 📄 App.test.js
├── 📄 index.jsx
├── 📄 reportWebVitals.js
├── 📄 setupTests.js
│
├── 📁 api
│ ├── 📄 auth.js
│ ├── 📄 comments.js
│ └── 📄 posts.js
│
├── 📁 assets
│ └── 📁 images
│ ├── 🖼️ logo.png
│ └── 🖼️ user.png
│
├── 📁 components
│ ├── 📄 TextEditor.jsx
│ │
│ ├── 📁 comments
│ │ ├── 📄 CommentForm.jsx
│ │ ├── 📄 CommentReply.jsx
│ │ ├── 📄 CommentsList.jsx
│ │ ├── 📄 CommentsReplyForm.jsx
│ │ └── 📄 CommentsReplyList.jsx
│ │
│ └── 📁 layouts
│ ├── 📄 Footer.jsx
│ ├── 📄 Layout.jsx
│ └── 📄 Navbar.jsx
│
├── 📁 context
│ └── 📄 AuthContext.jsx
│
├── 📁 pages
│ ├── 📁 default-set
│ │ └── 📄 NotFound.jsx
│ │
│ ├── 📁 protected
│ │ ├── 📄 HomePage.jsx
│ │ └── 📄 MyPage.jsx
│ │
│ └── 📁 public
│ ├── 📄 PostDetailPage.jsx
│ ├── 📄 PostFormPage.jsx
│ ├── 📄 PostListPage.jsx
│ ├── 📄 PublicHomePage.jsx
│ ├── 📄 SigninPage.jsx
│ ├── 📄 SignupPage.jsx
│ └── 📄 UserProfilePage.jsx
│
├── 📁 shared
│ ├── 📄 ProtectedRoute.jsx
│ └── 📄 Router.jsx
│
├── 📁 styles
│ ├── 📄 CommonContainer.jsx
│ ├── 📄 CustomToolbar.jsx
│ ├── 📄 GlobalStyle.jsx
│ └── 📄 theme.jsx
│
└── 📁 utils
├── 📄 calculator.js
└── 📄 data.js
```

<br>

# 1. Axios 설정

> 인증이 필요한 API 요청을 위해 Axios 인스턴스를 생성한 후, 회원가입, 로그인, 로그아웃 등 인증과 관련된 API 함수를 정의하자.

{% raw %}

```jsx
// src/api/auth.js
import axios from "axios";

const authAxios = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL, // 환경 변수에서 기본 URL을 설정
  withCredentials: true, // 쿠키를 자동으로 포함시키기 위한 설정
});

// Axios 인스턴스 생성 - 요청 전 에러 처리 위함
authAxios.interceptors.request.use(
  (config) => {
    return config;
  },
  (error) => {
    console.error("에러가 발생하였습니다. 문의해주세요.", error);
    return Promise.reject({ state: "ERROR", message: error.message });
  }
);

// 회원 가입
export const register = async (data) => {
  return await authAxios.post(`/members/new`, data);
};

// 로그인
export const login = async (data) => {
  return await authAxios.post(`/login`, data);
};

// 로그아웃
export const logout = async () => {
  return await authAxios.get(`/logout`);
};

// 프로필 조회
export const getProfile = async () => {
  return await authAxios.get(`/profile`);
};

// 프로필 닉네임 변경
export const updateNickname = async (newNickname) => {
  return await authAxios.post(`/profile/updateNickname`, { newNickname });
};

// 타 사용자 프로필 조회
export const getMembersProfile = async (memberId) => {
  return await authAxios.get(`/members/${memberId}/profile`);
};
```

{% endraw %}

<br><br>

# 2. AuthContext 생성

> 인증 상태를 관리하기 위해 AuthContext를 생성하자.

{% raw %}

```jsx
// src/context/AuthContext.jsx
import { getProfile } from "api/auth";
import { createContext, useContext, useEffect, useState } from "react";

const AuthContext = createContext();

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
        setUser(null); // 인증 실패 시 사용자 정보 초기화
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

export const useAuth = () => useContext(AuthContext); // AuthContext를 쉽게 사용할 수 있도록 하는 커스텀 훅
// export default AuthContext;
// const { user, login, logout, isSignIn } = useAuth(); 와 같이 사용 가능
```

{% endraw %}

<br><br>

# 3. ProtectedRoute.jsx 설정

> 인증이 필요한 페이지에 접근하기 위해 ProtectedRoute 컴포넌트를 설정하자.

{% raw %}

```jsx
// src/shared/ProtectedRoute.jsx
import { useAuth } from "context/AuthContext";
import { Navigate, Outlet, useLocation } from "react-router-dom";

const ProtectedRoute = () => {
  const { isSignIn } = useAuth();
  const location = useLocation(); // 현재 페이지의 경로 정보를 가져옴

  if (!isSignIn) {
    return <Navigate to="/sign-in" state={{ from: location }} replace />;
  }

  return <Outlet />;
};

export default ProtectedRoute;
```

{% endraw %}

<br><br>

# 4. 라우터 설정

> 라우트를 정의하고, 인증 여부에 따라 접근 가능한 경로를 설정하자.

{% raw %}

```jsx
// src/shared/Router.jsx
import Layout from "components/layouts/Layout";
import MyPage from "pages/protected/MyPage";
import PostFormPage from "pages/public/PostFormPage";
import SigninPage from "pages/public/SigninPage";
import SignupPage from "pages/public/SignupPage";
import {
  createBrowserRouter,
  Navigate,
  RouterProvider,
} from "react-router-dom";
import ProtectedRoute from "./ProtectedRoute";
import PostListPage from "pages/public/PostListPage";
import PostDetailPage from "pages/public/PostDetailPage";
import UserProfilePage from "pages/public/UserProfilePage";
import PublicHomePage from "pages/public/PublicHomePage";

const Router = () => {
  // 공통 라우트 설정
  const commonRoutes = [
    { path: "/", element: <PublicHomePage /> }, // 모든 사용자에게 기본 페이지로 제공
    { path: "/post-list", element: <PostListPage /> },
    { path: "/posts/:id", element: <PostDetailPage /> },
    { path: "/user-profile/:id", element: <UserProfilePage /> },
  ];

  // 비인증 사용자 전용 라우터 설정
  const notAuthenticatedRoutes = [
    { path: "/sign-in", element: <SigninPage /> },
    { path: "/sign-up", element: <SignupPage /> },
  ];

  // 인증 사용자 전용 라우트 설정
  const authenticatedRoutes = [
    {
      element: <ProtectedRoute />, // 보호된 라우트 적용
      children: [
        { path: "/my-page", element: <MyPage /> },
        { path: "/post-form", element: <PostFormPage /> },
      ],
    },
  ];

  // 404 페이지 라우트 설정
  const notFound = {
    path: "*",
    element: <Navigate to="/" />,
  };

  const router = createBrowserRouter([
    {
      element: <Layout />,
      children: [
        ...commonRoutes,
        ...notAuthenticatedRoutes,
        ...authenticatedRoutes,
      ],
    },
    notFound,
  ]);

  return <RouterProvider router={router} />;
};

export default Router;
```

{% endraw %}

<br><br>

# 5. App.jsx

> App 컴포넌트를 아래와 같이 설정해 주도록 하자.

{% raw %}

```jsx
// src/App.jsx
import { AuthProvider } from "context/AuthContext";
import Router from "shared/Router";

const App = () => {
  return (
    <AuthProvider>
      <Router />
    </AuthProvider>
  );
};

export default App;
```

{% endraw %}

<br><br>

# 6. Navbar 설정

> 사용자의 인증 상태에 따라 다르게 표시되게 아래와 같이 설정해주자.

```jsx
import { logout } from "api/auth";
import { useAuth } from "context/AuthContext";
import { NavLink } from "react-router-dom";
import styled from "styled-components";

const Navbar = () => {
  const { setUser, isSignIn } = useAuth();

  const hanldeLogout = async () => {
    try {
      const logoutConfirm = window.confirm("정말 로그아웃 하시겠습니까?");
      if (logoutConfirm) {
        await logout();
        alert("로그아웃 되었습니다.");
        setUser(null);
      }
    } catch (error) {
      console.log(error);
    }
  };

  return (
    <StNavbarContainer>
      <StNavLink to="/">로고</StNavLink>
      <StNav>
        {isSignIn ? (
          <>
            <StNavLink to="/my-page">마이페이지</StNavLink>
            <StNavLink to="/post-form">글 작성</StNavLink>
            <StNavLink to="/post-list">글 목록</StNavLink>
            <StLogout onClick={hanldeLogout}>로그아웃</StLogout>
          </>
        ) : (
          <>
            <StNavLink to="/post-list">글 목록</StNavLink>
            <StNavLink to="/my-page">마이페이지</StNavLink>
            <StNavLink to="/sign-in">로그인</StNavLink>
            <StNavLink to="/sign-up">회원가입</StNavLink>
          </>
        )}
      </StNav>
    </StNavbarContainer>
  );
};

export default Navbar;

const StNavbarContainer = styled.nav`
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
`;

const StNav = styled.div`
  display: flex;
  justify-content: end;
`;
const StNavLink = styled(NavLink)`
  margin: 0 6px;
  &.active {
    color: red;
    font-weight: bold;
  }
`;

const StLogout = styled.div`
  cursor: pointer;
`;
```

<br>
