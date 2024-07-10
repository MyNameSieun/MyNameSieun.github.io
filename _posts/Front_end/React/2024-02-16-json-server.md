---
title: "[React] json-server"
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

# 1. json-server 개요

## 1.1 json-server 개념

- json-server는 간단한 DB와 API 서버를 생성해주는 패키지이다.
- JSON 형식으로 데이터를 제공하는 간단한 서버를 만들어준다.
- json-server를 사용하면 실제 백엔드 서버가 없어도 데이터를 테스트하고 개발할 수 있어 효율적이고 빠른 개발을 도와준다.

<br>

## 1.2 json-server 설치

yarn 또는 npm을 이용해 설치 하면 된다. [공식문서](https://www.npmjs.com/package/json-server)

```
yarn global add json-server
```

<br>

그 후, db.json 파일을 만들어 아래와 같은 내용을 넣어주자. 이 json 파일을 db로 사용한다.

```js
{
  "posts": [
    { "id": "1", "title": "a title", "views": 100 },
    { "id": "2", "title": "another title", "views": 200 }
  ],
  "comments": [
    { "id": "1", "text": "a comment about post 1", "postId": "1" },
    { "id": "2", "text": "another comment about post 1", "postId": "1" }
  ],
  "profile": {
    "name": "typicode"
  }
}
```

<br><br>

# 2. json-server 사용하기

`json-server`가 간단한 패키지이긴 하나, 서버이다. 따라서 리액트와는 별개로 따로 실행을 해줘야 리액트와 json-server가 서로 통신 할 수 있다.

아래 명령을 통해 json-sever를 실행할 수 있다. http://localhost:3001/ 을 통해 접근이 가능하다.

```
yarn json-server --watch db.json --port 3001
```

<br>

json 파일에 있는 posts, comments, profile 라는 기본적인 값을 통해 아래와 같이 접근할 수 있다. (posts의 2번 id로 접근)

![](/assets/images/2024/2024-02-16-23-14-28.png)

<br>
