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

아래 명령을 통해 json-sever를 실행할 수 있다. `http://localhost:4000/` 을 통해 접근이 가능하다.

```
json-server --watch db.json --port 4000
```

<br>

json 파일에 있는 posts, comments, profile 라는 기본적인 값을 통해 아래와 같이 접근할 수 있다. (posts의 2번 id로 접근)

![](/assets/images/2024/2024-09-30-17-00-11.png)

<br><br>

# 3. 간단히 JSON 서버 시작하기

package.json 파일에 스크립트를 추가하여 JSON 서버를 간편하게 실행할 수 있다. 다음과 같은 내용을 package.json 파일의 scripts 섹션에 추가해 보자

```json
"scripts": {
  "serve": "json-server --watch db.json --port 4000"
}
```

이제 터미널에서 `yarn serve` 명령어를 입력하면, json-server가 db.json 파일을 감시(--watch)하고 4000번 포트에서 서버를 시작한다.

<br>

> ❓--watch란?

json-server를 사용할 때 --watch 옵션을 사용하면 지정한 JSON 파일의 변화를 실시간으로 감시하고, 파일이 변경될 때마다 서버 데이터를 자동으로 갱신할 수 있다.

<br>
