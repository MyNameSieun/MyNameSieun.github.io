---
title: "[React] React Qurey"
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

# 1. React Query 개요

## 1.1 React Query 개념

React Query는 데이터 Fetching, Caching, 동기화, 서버 데이터 업데이트 등의 작업을 간단하게 수행할 수 있도록 도와주는 라이브러리이다.

<br>

## 1.2 기존 미들웨어의 한계

다른 서버와의 API 통신과 비동기 데이터 관리를 위해 Redux-thunk, Redux-Saga 등 미들웨어를 채택할 수 있지만 다음과 같은 문제가 있다.

1. 보일러 플레이트 : 코드량이 너무 많다.
2. 규격화 문제 : Redux가 비동기 데이터 관리를 위한 전문 라이브러리가 아님(규격화 문제)

<br>

## 1.3 React Query 장점

[「if(kakao)2021 - 카카오페이 프론트엔드 개발자들이 React Query를 선택한 이유」 세줄요약 ](https://tech.kakaopay.com/post/react-query-1/)

1. React Query는 React Application에서 서버 상태를 불러오고, 캐싱하며, 지속적으로 동기화하고 업데이트하는 작업을 도와주는 라이브러리입니다.
2. 복잡하고 장황한 코드가 필요한 다른 데이터 불러오기 방식과 달리 React Component 내부에서 간단하고 직관적으로 API를 사용할 수 있습니다.
3. 더 나아가 React Query에서 제공하는 캐싱, Window Focus Refetching 등 다양한 기능을 활용하여 API 요청과 관련된 번잡한 작업 없이 “핵심 로직”에 집중할 수 있습니다.

즉, 사용하기 쉽고 직관적이기 때문에 사용한다.

<br>

## 1.4 주요 키워드

| 키워드             | 설명                                                                                                                                                                                                                                                             |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Query              | 데이터에 대한 요청을 의미한다. 주로 조회하는 데 사용되며, 데이터를 가져오는 작업과 비슷하다. (예: axios.get)                                                                                                                                                     |
| Mutation           | - 데이터를 변경하는 작업을 의미한다. 데이터의 추가, 수정, 삭제 등이 해당되며, CRUD 중에서 CUD(Create, Update, Delete)에 해당한다.<br>- axios의 경우 post, put, patch, delete 요청과 비슷하다.(예: axios.post, axios.put, axios.patch, axios.delete)              |
| Query Invalidation | Query를 무효화하는 작업을 의미한다. 서버의 데이터는 언제든지 변경될 수 있기 때문에, 가져온 Query가 최신 상태가 아닐 수 있다. 이런 경우, 기존의 Query를 무효화한 후 최신 데이터를 다시 가져오는 작업이 필요하며, 이 과정을 React Query에서는 자동으로 처리해준다. |

<br><br>

# 2. 구현하기

## 2.1 조회기능 구현

```js
yarn add axios
```

```js
yarn add react-query
```

<br>

## 2.2 추가 기능 구현

<br><br>

# 3. useQuery

<br>

# 4. mutations

<br>

# 참조

- https://www.nextree.io/react-query/

```

```
