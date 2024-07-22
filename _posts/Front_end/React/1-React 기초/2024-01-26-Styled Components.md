---
title: "[React] Styled Components"
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

# 1. Styled Components 개요

## 1.1 Css-in-Js

> Css-in-Js란 JavaScript로 CSS코드를 작성하는 방식을 말한다.

- 이 방식을 사용하기 위해 styled-components 컴포넌트를 설치해줘야한다.
- styled-components는 리액트에서 CSS-in-JS 방식으로 컴포넌트를 꾸밀수 있게 도와주는 패키지이다.

<br>

아래와 같이 입력하여 yarn 에서 styled-components 설치해주도록하자.

```shell
yarn add styled-components
```

<br>

## 1.2 styled-components

> styled-components를 사용하기 위해 꾸미고자 하는 컴포넌트를 SC의 방식대로 먼저 만들고, 그 안에 스타일 코드를 작성하면된다.

- `styled.` **뒤에는 html의 태그**가 온다.
  - div → `styled.div`
  - button → `styled.button`

```jsx
// src/App.js

import styled from "styled-components"; // (1) import하기

const App = () => {
  // (3) 만든 CS를 JSX에서 html 태그를 사용하듯이 사용
  return <StBox>박스</StBox>;
};

// (2) styled 키워드를 사용해서 SC 방식대로 컴포넌트를 만들기
const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid red;
  margin: 20px;
`;

export default App;
```

![](/assets/images/2024/2024-01-26-02-38-56.png)

<br><br>

# 2. 조건부 스타일링

## 2.1 조건부 스타일링 구현

> Styled Components을 사용하면 props를 통해서 부모 컴포넌트로부터 값을 전달받고, 조건문을 이용해서 조건부 스타일링을 할 수 있다.

```jsx
import styled from "styled-components";

const App = () => {
  return (
    <div>
      {/* (1) props를 통해 borderColor라는 값을 전달 */}
      <StBox borderColor="red">빨간 박스</StBox>
      <StBox borderColor="green">초록 박스</StBox>
      <StBox borderColor="blue">파랑 박스</StBox>
    </div>
  );
};

export default App;

const StBox = styled.div`
  width: 100px;
  height: 100px;
  border: 1px solid ${(props) => props.borderColor}; // (2) 부모 컴포넌트에서 보낸 props를 받아 사용
  margin: 20px;
`;
```

![](/assets/images/2024/2024-01-26-02-54-34.png)

<br>

부모 컴포넌트에서 보낸 props를 받아 사용하는 방법을 자세히 살펴보자.

```js
const StBox = styled.div`
  border: 1px solid ${(props) => props.borderColor};
`;
```

1. 자바스크립트 코드를 사용하기 위해 `${ }` 사용
2. 비어있는 화살표 함수 열어주기 `${( )=>{ }}`
3. 함수의 인자에서 props를 받아오고, props안에는 부모 컴포넌트에서 보낸 borderColor가 있는데 그것을 return하기

<br>

## 2.2 조건부 스타일링 실습 - button

<br>

> Props를 사용하는 Button 컴포넌트

styled-component에서만 사용되는 props라는 뜻으로 `$` 를 접두사로 붙혀주자.

```js
function App() {
  return (
    <div>
      <Button $primary>Primary Button</Button>
    </div>
  );
}

const Button = styled.button`
  background-color: ${(props) => (props.$primary ? "blue" : "gray")};
  color: white;

  &:hover {
    background-color: ${(props) => (props.$primary ? "darkblue" : "darkgray")};
  }
`;
```

<br><br>

# 3. Global Styles

> Global Styles(전역 스타일링)이란?
>
> 컴포넌트 내에서만 활용할 수 있는 styled components와 달리 공통적으로 들어가야 할 스타일을 적용할 때 사용하는 방법이 전역 스타일링이다.

<br>

아래는 컴포넌트 단위 스타일링이다.

```js
import styled from "styled-components";

function TestPage(props) {
  return (
    <Wrapper>
      <Title>{props.title}</Title>
      <Contents>{props.contents}</Contents>
    </Wrapper>
  );
}

const Title = styled.h1`
  font-family: "Helvetica", "Arial", sans-serif;
  line-height: 1.5;
  font-size: 1.5rem;
  margin: 0;
  margin-bottom: 8px;
`;

const Contents = styled.p`
  margin: 0;
  font-family: "Helvetica", "Arial", sans-serif;
  line-height: 1.5;
  font-size: 1rem;
`;

const Wrapper = styled.div`
  border: 1px solid black;
  border-radius: 8px;
  padding: 20px;
  margin: 16px auto;
  max-width: 400px;
`;

export default TestPage;
```

<br>

컴포넌트 단위 스타일링을 font와 line-height를 공통 요소라 가정하고 GlobalStyles을 적용해보자.

```jsx
// GlobalStyle.jsx
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
  body {
    font-family: "Helvetica", "Arial", sans-serif;
    line-height: 1.5;
  }
`;

export default GlobalStyle;
```

```jsx
// App.jsx
import GlobalStyle from "./GlobalStyle";
import BlogPost from "./BlogPost";

function App() {
  const title = "전역 스타일링 제목입니다.";
  const contents = "전역 스타일링 내용입니다.";
  return (
    <>
      <GlobalStyle />
      <BlogPost title={title} contents={contents} />
    </>
  );
}

export default App;
```

<br>
