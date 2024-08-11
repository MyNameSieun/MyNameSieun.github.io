---
title: "[React] Component"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 컴포넌트 개요

## 1.1 컴포넌트의 개념

① 컴포넌트란 리액트로 만들어진 앱을 이루는 최소한의 단위이며(벽돌), 화면의 특정 부분이 어떻게 생길지 정하는 <span style="color:indianred">선언체</span>이다.<br>
리액트에서 개발할 모든 애플리케이션은 컴포넌트라는 조각으로 구성된다.

② 즉, 컴포넌트를 통해 UI를 재사용이 가능한 개별적인 여러 조각으로 나누고, 각 조각을 개별적으로 살펴볼 수 있다. (독립적이다.)

③ 컴포넌트는 UI 구축 작업을 훨씬 쉽게 만들어준다.

④ 개념적으로 컴포넌트는 JS 함수와 유사하다.
“props”(input)라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 React 엘리먼트(output)를 반환한다.

컴포넌트를 어떻게 만들 수 있는지 알아보자!

## 1.2 함수형 컴포넌트

```js
// props라는 입력을 받음
// 화면에 어떻게 표현되는지를 기술하는 React 엘리먼츠를 반환(return)

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// 훨씬 쉬운 표현
function App() {
  return <div>hello</div>;
}
```

즉, 리액트 세계에서 말하는 컴포넌트는 함수이다. 따라서 컴포넌트를 만들고 싶다면 html을 return 하는 함수를 만들면 된다.

<br>

> 어떤 것을 컴포넌트로 만들면 좋을까?

① 반복적인 html을 축약할 때

② 큰 페이지들

③ 자주 변경되는 것들

<br>

## 1.3 컴포넌트 보는 방법

컴포넌트(함수) 코드를 볼 때는 영역을 나누어서 보면 조금 더 편하다.

```js
// import 영역 : 내가 필요한 파일을 가져옴
import logo from "./logo.svg";
import "./App.css";

function App() {
  // js 작성 가능한 영역
  const x = 1;
  function testFunc() {}

  return (
    // JSX(JS + XML(HTML) : 작스) 작성 가능한 영역
    // JS를 쓰려면 중괄호{} 를 써주면 된다.
    <div className="App">
      <header className="App-header">
        {testFunc}
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

// export 영역 : 내가 만든 컴포넌트를 밖으로 내보내는 영역
export default App;
```

① retrun문 안에는 JSX 문법을 쓸 수 있다.

② JSX 문법은 HTML과 비슷한 형태로 되어있다.

③ JSX 문법 안에서 JS를 쓰려면 중괄호{}로 묶어주면 된다.

<br><br>

# 2. 컴포넌트 만들기

> 함수를 정의하여 컴포넌트를 만들어보자

## 2.1 컴포넌트 만드는 규칙

① 컴포넌트를 만들 때 반드시 가장 첫 글자는 **대문자**여야 한다.

② 폴더는 소문자로 시작하는 카멜케이스로 작성하고, 컴포넌트를 만드는 파일은 대문자로 시작하는 카멜케이스로 이름을 짓는다.

<br>

## 2.2 실습

아래 코드를 복붙하자

{% raw %}

```js
import React from "react";
function App() {
  // <---- 자바스크립트 영역 ---->

  return (
    /* <---- HTML/JSX 영역  ---->*/
    <div
      style={{
        height: "100vh",
        display: " flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      {/* 이곳에 퀴즈를 위한 html 코드를 작성해 주세요 */}
    </div>
  );
}

export default App;
```

{% endraw %}

<br>

“HTML 영역” 안에서 알맞은 html 태그를 작성해 아래와 같은 화면을 만든 후, “클릭!” 버튼을 눌렀을 때 alert창이 나타나게 해보자.<br><br>
![Alt text](../../../assets/images/2024/2024-01-17-16-51-44.png)

{% raw %}

```js
import React from "react";
function App() {
  // <---- 자바스크립트 영역 ---->
  const onClickBtnHander = () => alert("클릭!");

  return (
    /* <---- HTML/JSX 영역  ---->*/
    <div
      style={{
        // 객체로 구성
        height: "100vh",
        display: " flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      <span>이것은 내가 만든 APP 컴포넌트입니다.</span>
      <button onClick={onClickBtnHander}>클릭</button>
    </div>
  );
}

export default App;
```

{% endraw %}

<br><br>

# 3. 부모-자식 컴포넌트

## 3.1 컴포넌트 안에 컴포넌트 넣기

아래와 같이 컴포넌트 안에 컴포넌트를 넣을 수 있다.

```js
// src/App.js
import React from "react";

// 자식 컴포넌트
function Child() {
  return <div>자식 컴포넌트</div>;
}

// 부모 컴포넌트
function App() {
  return <Child />;
}

export default App;
```

<br>

## 3.2 실습

현재 App.js에 작성된 코드를 모두 지운 후, `App.js`에 3개의 컴포넌트를 만들고 할아버지, 엄마, 자식 컴포넌트를 만들어보고 서로 연결시켜보자.

```js
import React from "react";

function Child() {
  return <div>자식입니다.</div>;
}

function Mother() {
  return <Child />;
}

function GrandFather() {
  return <Mother />;
}

function App() {
  return <GrandFather />; // "자식입니다."
}

export default App;
```

<br>
