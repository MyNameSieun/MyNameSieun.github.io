---
title: "[React] React GlobalStyle에서 reset.css 사용하기"
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

## React에서 reset.css 사용하기

> 설치하기

```shell
// yarn
$ yarn add styled-reset

// npm
$ npm i styled-reset
```

<br>

> 코드 적용

GlobalStyle.js 파일에 아래와 같이 적어주기

```js
// css 초기화
import reset from "styled-reset";
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
 ${reset}
  body {
    font-family: "Helvetica", "Arial", sans-serif;
    background-color:#F5F5F5;
    height:100vh;
  }
  a{text-decoration:none;}
  a:visited { color:black; }
`;

export default GlobalStyle;
```

<br>
