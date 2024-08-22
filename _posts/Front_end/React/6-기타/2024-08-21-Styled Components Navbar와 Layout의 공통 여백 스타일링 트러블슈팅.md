---
title: "[React] Styled Components Navbar와 Layout의 공통 여백 스타일링 트러블슈팅"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 트러블 슈팅💫

## 발생한 문제 🤦‍♀️

> Navbar와 Layout의 양쪽 여백을 동일하게 유지하는 과정에서, 각 요소에 배경색을 적용할 때 문제가 발생했다.

<br>

### 시도한 내용 🦹‍♀️

> 처음에는 `CommonLayout.jsx`이라는 파일을 생성하여, router에서 Navbar와 Layout을 감싸는 방식으로 여백을 조정하였다.

```jsx
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* 공통 레이아웃으로 감싸고, 조건에 따라 하위 레이아웃 적용 */}
        <Route element={<CommonLayout />}>
          {/* 로그인 여부와 상관없는 페이지 */}
          <Route element={<Layout />}>
            <Route path="/" element={<PublicHomePage />} />
            <Route path="sign-in" element={<SignInPage />} />
            <Route path="sign-up" element={<SignUpPage />} />
          </Route>

          {/* 로그인해야만 접근 가능한 페이지 */}
          <Route element={<AuthLayout />}>
            <Route path="/home" element={<HomePage />} />
            <Route path="/user/:userId" element={<UserProfilePage />} />
          </Route>

          {/* 404 Not Found 페이지 */}
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

> Outlet을 통해 CommonLayout 하위의 컴포넌트들이 공통된 스타일을 적용할 수 있게 하였다.

```jsx
import styled from "styled-components";
import { Outlet } from "react-router-dom";

const CommonLayout = () => {
  return (
    <StContainer>
      <Outlet />
    </StContainer>
  );
};

export default CommonLayout;

const StContainer = styled.div`
  max-width: 1200px;
  margin: 0 auto;
`;
```

```jsx
import { Outlet } from "react-router-dom";
import Navbar from "./Navbar";
import styled from "styled-components";

const Layout = () => {
  return (
    <StLayout>
      <Navbar />
      <StContainer>
        <p>로그인 여부와 상관없이 접근 가능한 페이지입니다.</p>
        <main>
          <Outlet />
        </main>
      </StContainer>
    </StLayout>
  );
};

export default Layout;

const StLayout = styled.nav`
  background-color: #e2e2e2;
  border-bottom: 1px solid #ddd;
`;

const StContainer = styled.div`
  flex-direction: column;
  min-height: 100vh;
`;
```

<br>

> 그러나 이 방법은 CommonLayout을 사용하여 Navbar와 Layout을 감쌌을 때 양쪽 여백이 적용되었지만, background-color가 여백까지 적용되지 않는 문제가 있었다.

![](/assets/images/2024/2024-08-21-15-56-13.png)

<br>

### 해결 방안 💁‍♀️

> 공통 스타일을 정의하여 재사용

문제를 해결하기 위해 `CommonContainer.jsx`를 생성하고, 이를 Navbar와 Layout에서 재사용하는 방식으로 접근하였다.

① CommonContainer 정의

```jsx
// src/styles/CommonStyles.jsx
import styled from "styled-components";

export const CommonContainer = styled.div`
  display: flex;
  max-width: 1200px;
  margin: 0 auto;
`;
```

<br>

② Navbar와 Layout에서 사용

```jsx
import { NavLink, useNavigate } from "react-router-dom";
import styled from "styled-components";
import { CommonContainer } from "styles/CommonStyles"; // import

const Navbar = () => {
  return (
    <StNav>
      <StNavContainer>
        <NavLink to="/">Home</NavLink>
        <NavLink to="/sign-in">로그인</NavLink>
        <NavLink to="/sign-up">회원가입</NavLink>
      </StNavContainer>
    </StNav>
  );
};

export default Navbar;

const StNav = styled.nav`
  background-color: #f8f9fa;
  padding: 10px 20px;
  border-bottom: 1px solid #ddd;
`;

const StNavContainer = styled(CommonContainer)`
  justify-content: space-between;
  align-items: center;
`;
```

```jsx
import { Outlet } from "react-router-dom";
import Navbar from "./Navbar";
import styled from "styled-components";
import { CommonContainer } from "styles/CommonStyles"; // import

const Layout = () => {
  return (
    <>
      <Navbar />

      <StLayout>
        <StContainer>
          <p>로그인 여부와 상관없이 접근 가능한 페이지입니다.</p>
          <main>
            <Outlet />
          </main>
        </StContainer>
      </StLayout>
    </>
  );
};

export default Layout;

const StLayout = styled.nav`
  background-color: #e2e2e2;
  border-bottom: 1px solid #ddd;
`;

const StContainer = styled(CommonContainer)`
  flex-direction: column;
  min-height: 100vh;
`;
```

<br>

> 이렇게 background-color를 다르게 적용하고, 여백은 공통으로 유지할 수 있게 되었다!

![](/assets/images/2024/2024-08-21-15-36-04.png)

<br>
