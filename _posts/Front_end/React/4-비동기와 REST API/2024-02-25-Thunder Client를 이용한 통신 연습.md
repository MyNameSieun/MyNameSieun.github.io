---
title: "[React] Thunder Client를 이용한 통신 연습"
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

# 0. 개요

> 회원가입, 로그인, 회원정보 확인, 프로필 이미지 변경 API를 지원하는 JWT 인증 서버를 활용하여 서버통신을 연습해보자

서버 API_URL : [https://moneyfulpublicpolicy.co.kr](https://moneyfulpublicpolicy.co.kr/)

<br>

> Thunder Client⚡ 설치하기

- Thunder Client란 vsCode내에서 HTTP 요청을 생성하고 테스트 할 수 있게 해주는 HTTP 클라이언트 확장 프로그램이다.
- HTTP 요청을 보내고 응답을 받는데 필요한 모든 기능을 가지고 있어 쉽게 API를 테스트하고 디버깅 할 수 있게 도와준다.

![](/assets/images/2024/2024-02-26-00-55-59.png)

<br>

# 1. API 테스트하기

⚠️ Request, Response의 방식은 서버마다 다르므로 각 서버의 API 문서를 확인하여 요청과 응답 방식을 확인해야 한다!

## 1. 회원가입

아이디, 비밀번호, 닉네임으로 DB에 본인의 회원정보를 저장한다.

> Request

- Method → `POST`<br>
- URL PATH → `/register`
- Body
  ```json
  JSON
  {
      "id": "유저 아이디",
      "password": "유저 비밀번호",
      "nickname": "유저 닉네임"
  }
  ```

<br>

> Response

```json
{
  "message": "회원가입 완료",
  "success": true
}
```

![](/assets/images/2024/2024-02-26-01-15-06.png)

<br>

## 2. 로그인

아이디와 비밀번호가 DB에 있는 회원정보와 일치하면 accessToken, userId, avatar, nickname 총 4가지 유저정보를 응답해준다.

> Request

- Method → `POST`
- URL PATH → `/login`
- Body
  ```json
  JSON
  {
    "id": "유저 아이디",
    "password": "유저 비밀번호"
  }
  ```

![](/assets/images/2024/2024-02-26-01-16-10.png)

<br>

로그인 정보는 서버와 클라이언트 둘 다 관리를 해야한다. 유효한 토큰을 가지고 있는지 확인하기 위함이다.

- 서버는 유효한 토큰을 가지고 있는지 확인해야한다.
- 클라이언트는 서버 낭비 방지 (토큰의 유효 기간이 만료되면 서버에게 보낼 필요가 없음) 를 하기 위해 토큰 관리를 해야한다.

![](/assets/images/2024/2024-02-26-01-32-32.png)

<br>

> Query string(선택)

- cessToken 유효시간 조정을 위한 query string
  - query string 없이 path로만 요청 시 기본 1시간
  - query string (expiresIn) 으로 시간 기입 시 해당 시간대로 토큰 유효시간 조정가능
  - expiresIn : 시간 단위를 붙인 문자열 ex) 10s 10m 10h
  - 토큰만료 시 로그아웃 처리되는 로직시 사용 가능

```json
/login?expiresIn=10m

// 유효시간 10분인 accessToken 요청
```

<br>

> Response

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6ImFiY2FiYyIsImlhdCI6MTcwMDgxNDQyMCwiZXhwIjoxNzAwODE4MDIwfQ.8hWOHHEzDPzumnqCU7jyoi3zFhr-HNZvC7_pzBfOeuU",
  "userId": "유저 아이디",
  "success": true,
  "avatar": "프로필 이미지",
  "nickname": "유저 닉네임"
}
```

<br>

## 3. 회원정보 확인

accessToken이 유효한 경우, 비밀번호를 제외한 본인의 회원정보를 응답해 준다.

```js
// authorization 속성 정의
const response = await axios.get(`${BASE_URL}/user`, {
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${accessToken}`,
  },
});
```

<br>

> Request

- Method → `GET`
- URL PATH → `/user`
- Header

  ```json
  {
    "Authorization": "Bearer AccessToken"
  }
  ```

<br>

> Response

```json
{
  "id": "사용자 아이디",
  "nickname": "사용자 닉네임",
  "avatar": null,
  "success": true
}
```

![](/assets/images/2024/2024-02-26-01-41-35.png)

<br>

## 4. 프로필 변경

accessToken이 유효한 경우, 프로필 이미지 또는 닉네임을 FormData을 통해 요청하면 변경 완료된 이미지 URL과 닉네임을 응답해준다.

```js
// 이미지파일을 FormData에 담는 방법

const formData = new FormData();
// avatar와 nickname 중 하나 또느 모두 변경 가능
formData.append("avatar", imgFile);
formData.append("nickname", nickname);

// 요청 시 Content-Type에 유의
const response = await axios.patch(`${BASE_URL}/profile`, formData, {
  headers: {
    "Content-Type": "multipart/form-data",
    Authorization: `Bearer ${accessToken}`,
  },
});
```

<br>

> Request

- Method → `PATCH`
- URL PATH → `/profile`
- Header
  ```json
  {
    "Authorization": "Bearer AccessToken"
  }
  ```
- Body
  ```json
  FORM
  {
  	"avatar": [이미지파일],
  	"nickname": "변경할 닉네임"
  }
  ```

<br>

> Response

```json
{
  "avatar": "변경된 이미지 URL",
  "nickname": "변경된 닉네임",
  "message": "프로필이 업데이트되었습니다.",
  "success": true
}
```

![](/assets/images/2024/2024-02-26-03-13-41.png)

<br>
