---
title: "[React] Component"
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

# ▶ 컴포넌트란?

> 컴포넌트란 리액트로 만들어진 앱을 이루는 최소한의 단위이며 리액트의 꽃이라 할 수 있다!
> 컴포넌트를 어떻게 만들 수 있는지 알아보자!

<br>

App.js 폴더를 열어 아래와 같이 코드를 작성해주었다.

```jsx
import logo from "./logo.svg";
import "./App.css";

function App() {
  return (
    <div className="App">
      <div></div>
      <header>
        <h1>리액트 공부하기</h1>
      </header>
      <nav>
        <ul>
          <li>2023.08.04</li>
          <li>드디어 리액트를 시작하다!</li>
          <li>신난다! ☆*: .｡. o(≧▽≦)o .｡.:*☆</li>
        </ul>
      </nav>
    </div>
  );
}

export default App;
```

<br>

## ▷ 컴포넌트 만들기

> 함수를 정의하여 컴포넌트를 만들어보자
> (⚠️ 컴포넌트를 만들 때 반드시 대문자로 시작해야한다.)

```jsx
import logo from "./logo.svg";
import "./App.css";

// 컴포넌트 생성
function Header() {
  return (
    <header>
      <h1>리액트 공부하기</h1>
    </header>
  );
}

function App() {
  // 생략
}

export default App;
```

<br>

## ▷ 컴포넌트 사용하기

> App( )에 우리가 방금 생성한 컴포넌트의 태그(Header)를 넣어 컴포넌트를 사용해보자!

```jsx
import logo from "./logo.svg";
import "./App.css";

// 컴포넌트 생성
function Header() {
  return (
    <header>
      <h1>리액트 공부하기</h1>
    </header>
  );
}

function App() {
  return (
    <div className="App">
      // 컴포넌트 사용
      <Header></Header>
      // 생략
    </div>
  );
}

export default App;
```

<br>

위 두 코드는 출력 결과가 똑같이 나오지만 컴포넌트를 사용했을 때 훨씬 코드를 간결하게 작성할 수 있다.
즉, 컴포넌트를 만드는 기술인 리액트 덕분에 여러 태그들을 독립된 부품으로 만들 수 있어 생산성을 획기적으로 끌어 올릴 수 있다!

<br>

---

<br>

# 📎참조

- 생활코딩 React 2022년 개정판 강의
- https://goddaehee.tistory.com/299
