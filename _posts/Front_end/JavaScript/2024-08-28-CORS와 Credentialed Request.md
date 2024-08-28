---
title: "[JS] CORS와 Credentialed Request"
categories: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. CORS (Cross-Origin Resource Sharing)

> CORS는 웹 브라우저가 다른 출처의 리소스에 접근할 수 있도록 허용하는 메커니즘이다.

- 기본적으로 웹 브라우저는 보안상의 이유로 같은 출처(프로토콜, 도메인, 포트 번호를 포함한 URL)의 리소스만 접근할 수 있도록 제한한다.
- 예를 들어, `http://localhost:4000`에서 `http://localhost:8080`의 요청은 동일 출처 정책(Same-Origin Policy)에 의해 차단된다.

➡️ 즉, 클라이언트가 서버의 리소스에 접근하려면 CORS를 통해 접근 권한을 허용해야한다.<br>
➡️ CORS를 사용하면 서버가 특정 출처의 요청을 허용할 수 있도록 설정할 수 있다.

<br><br>

# 2. Credentialed Request

Credentialed Request는 인증 정보(쿠키, HTTP 인증 헤더 등)가 포함된 요청을 의미한다. 서버와 클라이언트 모두 CORS 설정을 통해 자격 증명을 처리하도록 설정을 해야한다.

## 2.1 서버(Express) 설정

```js
// Express.js 서버
app.use(
  cors({
    origin: "*", // 출처 허용 옵션
    credentials: true, // 자격 증명 허용
  })
);
```

<br>

## 2.2 클라이언트 설정

> Axios에서 자격 증명 포함 설정(택 1)

① Axios 전역 설정<br>
: Axios 인스턴스의 기본 설정을 통해 모든 요청에 대해 자격 증명을 포함시키는 방법이다.

```jsx
// 1. axios 전역 설정
axios.defaults.withCredentials = true; // withCredentials 전역 설정
```

<br>

② Axios 요청 메소드의 옵션 객체로 설정<br>
: 특정 요청에서만 자격 증명을 포함시키는 방법이다.

```jsx
// 2. axios 옵션 객체로 넣기
axios.get("https://example.com/login", {
  withCredentials: true,
});
```

<br>

> Fetch API에서 자격 증명 포함 설정

Fetch API를 사용하여 자격 증명을 포함한 요청을 보낼 때, credentials 옵션을 사용하여 설정한다.

```jsx
fetch("https://example.com:1234/users/login", {
  method: "POST",
  credentials: "include", // 클라이언트와 서버가 통신할때 쿠키 값을 공유하겠다는 설정
});
```

<br>

# 참조

- [https://inpa.tistory.com/entry/AXIOS-📚-CORS-쿠키-전송withCredentials-옵션](https://inpa.tistory.com/entry/AXIOS-%F0%9F%93%9A-CORS-%EC%BF%A0%ED%82%A4-%EC%A0%84%EC%86%A1withCredentials-%EC%98%B5%EC%85%98)

<br>
