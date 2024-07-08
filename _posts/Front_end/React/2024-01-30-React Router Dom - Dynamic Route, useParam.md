---
title: "[React] React Router Dom - Dynamic Route, useParam"
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

> Dynamic route 기법과 useParam hook을 이용하여 동적 라우팅을 구현해보자

# 1. Dynamic Route 개요

## 1.1 Dynamic Route 개념

Dynamic Route란, 동적 라우팅이라고도 말하는데 pa하게끔 구현하는 방법을 말한다.

동적 라우트에서는 경로에 따라 컴포넌트를 동적으로 할당하고, 경로의 일부를 매개변수로 사용하여 컴포넌트를 렌더링하는 것이 가능하다.

즉, path에 유동적인 값을 넣어서 특정 페이지로 이동하게끔 할 수 있는 것이다.

동적 라우트를 사용하면 /users/1, /users/2, /users/3과 같은 다양한 사용자 프로필을 동일한 컴포넌트로 처리할 수 있다.

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

<br><br>

# 2. Dynamic Routes와 useParam

## 2.1 query parameter 조회하기

> useParams 이라는 훅을 사용해서 `Dynamic Routes` 에서 path에 입력된 id 값을 가져와보자<br>

useParams 은 path의 있는 id 값을 조회할 수 있게 해주는 훅이다.

Dynamic Routes를 사용하면 element에 설정된 같은 컴포넌트를 렌더링하게 되지만, useParam을 이용하면 같은 컴포넌트를 렌더링하더라도 각각의 고유한 `id` 값을 조회할 수 있다.

```jsx
<Route path="/info/:id" element={<Info />} />>
```

즉, works/1로 이동하면 1 이라는 값을 주고, works/100으로 이동하면 100 이라는 값을 사용할 수 있게 해주는 것이다.

<br>

> useParams를 찍어볼까?

```js
// Detail.js
import { useParams } from "react-router-dom";

function Detail() {
  const { id } = useParams();

  // id 변수에는 현재 URL에서 추출한 동적 값이 들어 있다.
  console.log("Letter ID:", id);

  return <div>32432</div>;
}

export default Detail;
```

![](/assets/images/2024/2024-02-05-02-24-34.png)
![](/assets/images/2024/2024-02-05-02-20-51.png)

<br>

> works/1이런식으로 해당 번호를 클릭할 때 세부 페이지로 이동해보도록 하자

① Router.jsx 이동해서 Route를 설정하자

```jsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "pages/Home";
import About from "pages/About";
import Contact from "pages/Contact";
import Info from "pages/Info";

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        {/* 이 부분 추가 */}
        <Route path="/info/:id" element={<Info />} />>
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

이전과는 다르게 path에 `info/:id` 라고 path가 들어간다.

`:id` 라는 것이 바로 "동적인 값을 받겠다."라는 의미이다.

<br>

② info.json

```jsx
[
  {
    id: 1,
    textsub: "졸업학점 계산기",
    textContent: " 졸업학점 계산기졸업학점 계산기졸업학점 계산기.",
    infoImage: "images/3d-calculator.png",
    infoLink: "",
  },
  {
    id: 2,
    textsub: "학교 근처 맛집",
    textContent:
      "학교 근처 맛집학교 근처 맛집학교 근처 맛집학교 근처 맛집학교 근처 맛집",
    infoImage: "images/udon.png",
    infoLink: "",
  },
];
```

<br>

③ Info.jsx 컴포넌트 변경

이제 info id에 따라 페이지가 출력되게 된다.

```jsx
import styled from "styled-components";
import { useNavigate, useParams } from "react-router-dom";
import info from "info.json";

const Info = () => {
  const navigate = useNavigate();
  const { id } = useParams();

  return (
    <InfoLayout>
      <InfoBox>
        <InfoImage src="images/leftArrow.png" onClick={() => navigate(-1)} />
        <InfoText>유용한 정보 /</InfoText>
      </InfoBox>
      <TextContent>
        {info
          .filter((item) => item.id === parseInt(id))
          .map((item) => {
            return (
              <div key={item.id}>
                <InfoTextSub>{item.textsub}</InfoTextSub>
                <TextContent>{item.textContent}</TextContent>
              </div>
            );
          })}
      </TextContent>
    </InfoLayout>
  );
};

export default Info;
```

<br>

## 2.2 뒤로가기/앞으로가기 구현

```js
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

<button onClick={() => navigate(-1)}>뒤로가기</button>
<button onClick={() => navigate(1)}>앞으로가기</button>
```

<br>
