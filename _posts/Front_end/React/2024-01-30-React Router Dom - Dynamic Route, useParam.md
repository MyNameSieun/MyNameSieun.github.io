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
        <Route path="/works/1" element={<Works />} />
        <Route path="/works/2" element={<Works />} />
        <Route path="/works/3" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};
```

위 코드를 react-router-dom에서 지원하는 Dynamic Routes 기능을 이용해서 간결하게 동적으로 변하는 페이지를 처리해보자.

<br><br>

# 2. Dynamic Routes와 useParam

## 2.1 query parameter 조회하기

useParams 이라는 훅을 사용해서 `Dynamic Routes` 에서 path에 입력된 id 값을 가져와보자<br>
useParams 은 path의 있는 id 값을 조회할 수 있게 해주는 훅이다.

Dynamic Routes를 사용하면 element에 설정된 같은 컴포넌트를 렌더링하게 된다.

```jsx
<Route path="works/:id" element={<Work />} />
```

하지만 useParam을 이용하면 같은 컴포넌트를 렌더링하더라도 각각의 고유한 `id` 값을 조회할 수 있다.

즉, works/1로 이동하면 1 이라는 값을 주고, works/100으로 이동하면 100 이라는 값을 사용할 수 있게 해주는 것이다.

```jsx
// src/pages/Works.js
import React from "react";
import { Link, useParams } from "react-router-dom";

const data = [
  { id: 1, todo: "리액트 배우기" },
  { id: 2, todo: "노드 배우기" },
  { id: 3, todo: "자바스크립트 배우기" },
  { id: 4, todo: "파이어 베이스 배우기" },
  { id: 5, todo: "넥스트 배우기" },
  { id: 6, todo: "HTTP 프로토콜 배우기" },
];

function Works() {
  return (
    <div>
      {data.map((item) => {
        return (
          <div key={item.id}>
            {item.id}
            {item.todo}
          </div>
        );
      })}
    </div>
  );
}

export default Works;
```

![](/assets/images/2024/2024-01-30-12-08-53.png)

<br>

이제 works/1이런식으로 해당 번호를 클릭할 때 세부 페이지로 이동해보도록하자

<br>

work 컴포넌트 생성

```jsx
// src/pages/Work.js

import React from "react";
import { useParams } from "react-router-dom";

const data = [
  { id: 1, todo: "리액트 배우기" },
  { id: 2, todo: "노드 배우기" },
  { id: 3, todo: "자바스크립트 배우기" },
  { id: 4, todo: "파이어 베이스 배우기" },
  { id: 5, todo: "넥스트 배우기" },
  { id: 6, todo: "HTTP 프로토콜 배우기" },
];

function Work() {
  const param = useParams();

  const work = data.find((work) => work.id === parseInt(param.id));

  return <div>{work.todo}</div>;
}

export default Work;
```

<br>

그리고 이제 Router.jsx 이동해서 아래 코드를 추가하자

```jsx
import React from "react";
import { BrowserRouter, Route, Routes } from "react-router-dom";
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
        {/* 이 부분 추가 */}
        <Route path="works/:id" element={<Works />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

<br>

이전과는 다르게 path에 `works/:id` 라고 path가 들어간다.

`:id` 라는 것이 바로 동적인 값을 받겠다라는 의미이다.

그래서 works/1 로 이동해도 `<Work />` 로 이동하고, `works/2`, `works/3` …. `works/100` 모두 `<Work />`하게 되는 것이다.

<br>

이제 하위 페이지로 넘어갈 수 있게 하기 위해 Link 태그를 사용하자

```jsx
// src/pages/Works.js

import React from "react";
import { Link, useParams } from "react-router-dom";

const data = [
  { id: 1, todo: "리액트 배우기" },
  { id: 2, todo: "노드 배우기" },
  { id: 3, todo: "자바스크립트 배우기" },
  { id: 4, todo: "파이어 베이스 배우기" },
  { id: 5, todo: "넥스트 배우기" },
  { id: 6, todo: "HTTP 프로토콜 배우기" },
];

function Works() {
  return (
    <div>
      {data.map((item) => {
        return (
          <div key={item.id}>
            {item.id}
            {/* 아래부분 추가 */}
            <Link to={`/works/${item.id}`}>{item.todo}</Link>
          </div>
        );
      })}
    </div>
  );
}

export default Works;
```

하이퍼링크가 생긴 것을 확인할 수 있다.

![](/assets/images/2024/2024-01-30-12-18-59.png)

<br>

이제 works에 id에 따라 페이지를 출력해보도록 하자.

Works.jsx에 있는 아래 코드를 shared/data.js 파일로 옮겨주도록 하자.

```jsx
// src/shared/data.js

// export
export const data = [
  { id: 1, todo: "리액트 배우기" },
  { id: 2, todo: "노드 배우기" },
  { id: 3, todo: "자바스크립트 배우기" },
  { id: 4, todo: "파이어 베이스 배우기" },
  { id: 5, todo: "넥스트 배우기" },
  { id: 6, todo: "HTTP 프로토콜 배우기" },
];
```

```jsx
// src/pages/Works.js

// import
import { data } from "../shared/data";
```

<br>

이제 Work 컴포넌트를 생성하자.

```jsx
// src/pages/Work.js

import React from "react";
import { useParams } from "react-router-dom";
import { data } from "../shared/data"; // import

function Work() {
  const param = useParams();

  // find로 어떤 todo인지 찾아보기
  const work = data.find((work) => work.id === parseInt(param.id));

  return (
    <div>
      <h3>할 일</h3>
      {work.todo}
    </div>
  );
}

export default Work;
```

```jsx
// Router.js
import React from "react";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from "../pages/Home";
import About from "../pages/About";
import Contact from "../pages/Contact";
import Works from "../pages/Works";
import Work from "../pages/Work"; // import

const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
        <Route path="works" element={<Works />} />
        {/* 아래 코드 추가 */}
        <Route path="works/:id" element={<Work />} />
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

잘 나오는 걸 볼 수 있다!

![](/assets/images/2024/2024-01-30-13-07-56.png)

<br>

> 아래처럼 요소 클릭시 네비게이션을 사용하여 페이지를 이동할 수 있다.

```js
// Router.js
import Detail from "components/Detail"; // import
<Route path="/detail/:id" element={<Detail />} />;
```

```js
import { useNavigate } from "react-router-dom"; //import

function ListLetter({ activePeople, letters }) {
  const navigate = useNavigate(); // useNavigate 사용
  return (
    <ListLetterContainer>
      {letters
        .filter(function (letter) {
          return letter.writedTo === activePeople;
        })
        .map(function (letter) {
          return (
            <LetterCard
              key={letter.id}
              onClick={() => navigate(`/detail/${letter.id}`)}
            ></LetterCard>
          );
        })}
    </ListLetterContainer>
  );
}
```

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

## 2.2 뒤로가기/앞으로가기 구현

```js
import { useNavigate } from "react-router-dom";

const navigate = useNavigate();

<button onClick={() => navigate(-1)}>뒤로가기</button>
<button onClick={() => navigate(1)}>앞으로가기</button>
```

<br>
