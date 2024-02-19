---
title: "[React] axios, json-server를 이용한 통신 연습"
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

**비동기 통신 - axios와 interceptor**를 공부하고 작성하는 글입니다.
{: .notice--danger}

# 1. 통신 연습

## 1.1 기초 세팅

먼저 json-server와 axios를 설치하자

```
yarn add axios
yarn add json-server
```

<br>

그리고 db.json 파일을 생성하자

```json
{
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "johndoe@example.com"
    },
    {
      "id": 2,
      "name": "Jane Doe",
      "email": "janedoe@example.com"
    }
  ],
  "posts": [
    {
      "id": 1,
      "userId": 1,
      "title": "John's first post",
      "body": "This is the content of John's first post."
    },
    {
      "id": 2,
      "userId": 1,
      "title": "John's second post",
      "body": "This is the content of John's second post."
    },
    {
      "id": 3,
      "userId": 2,
      "title": "Jane's first post",
      "body": "This is the content of Jane's first post."
    }
  ]
}
```

<br>

json-server을 실행하면 된다.

```
npx json-server db.json --port [원하는 포트]
```

<br>

다음 db.json 파일을 이용하여 백엔드를 세팅하자

```json
{
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "johndoe@example.com"
    },
    {
      "id": 2,
      "name": "Jane Doe",
      "email": "janedoe@example.com"
    }
  ],
  "posts": [
    {
      "id": 1,
      "userId": 1,
      "title": "John's first post",
      "body": "This is the content of John's first post."
    },
    {
      "id": 2,
      "userId": 1,
      "title": "John's second post",
      "body": "This is the content of John's second post."
    },
    {
      "id": 3,
      "userId": 2,
      "title": "Jane's first post",
      "body": "This is the content of Jane's first post."
    }
  ]
}
```

<br>

## 1.2 연습 문제

> 본격적으로 통신 연습을 진행해보자.

먼저, App.jsx의 내용은 변경하지 않고 그대로 붙여넣자.

```js
// src > App.jsx
import React, { useEffect, useState } from "react";
import api from "./api";

function App() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await api.get("/posts");
      } catch (error) {
        console.error("There was an error!", error);
      }
    };

    fetchPosts();
  }, []);

  return (
    <div>
      <h1>Posts</h1>
      {posts.map((post) => (
        <div key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.body}</p>
        </div>
      ))}
    </div>
  );
}

export default App;
```

<br>

그 다음, api.js 파일을 src 밑에 생성한 후 내용을 채워보자.

아래의 조건을 만족해야한다.

> 1. 기초 URL이 json-server를 가리켜야한다.
> 2. 요청시 항상 console.log("요청합니다."); 를 출력해야한다.
> 3. 응답시 항상 console.log("응답입니다."); 를 출력해야한다.

<br>

아래의 코드를 베이스로 조건을 충족시키면 된다.

```js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create();

// 요청 인터셉터 추가
api.interceptors.request.use();

// 응답 인터셉터 추가
api.interceptors.response.use();

export default api;
```

<br>

## 1.3 문제 풀이

처음에 내가 작성한 코드는 아래와 같다.

```js
// api.js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create({
  baseURL: "http://localhost:3001",
});

// 요청 인터셉터 추가
api.interceptors.request.use(console.log("요청합니다."));

// 응답 인터셉터 추가
api.interceptors.response.use(console.log("응답합니다."));

export default api;
```

<br>

초기 작성한 코드는 요청 인터셉터와 응답 인터셉터의 위치를 바꿔주었을 때, 응답이 먼저 되고 그 후 요청이 되는 문제가 발생했다.

```js
// 응답 인터셉터 추가
api.interceptors.response.use(console.log("응답합니다."));

// 요청 인터셉터 추가
api.interceptors.request.use(console.log("요청합니다."));
```

![](/assets/images/2024/2024-02-20-01-06-58.png)

<br>

interceptors.request.use 및 interceptors.response.use에서 사용하는 함수는 비동기 함수여야한다.<br> 따라서 console.log를 직접 전달하는 대신 함수를 전달하는 방식으로 바꿔주었다.

```js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create({
  baseURL: "http://localhost:3001",
});

// 응답 인터셉터 추가
api.interceptors.response.use(function (response) {
  console.log("응답합니다.");
  return response;
});

// 요청 인터셉터 추가
api.interceptors.request.use(function (request) {
  console.log("요청합니다");
  return request;
});

export default api;
```

이젠 응답 인터셉터와 요청 인터셉터의 위치를 바꿔줘도 정상적으로 응답 먼저 되는 것을 확인할 수 있다!

<br>
