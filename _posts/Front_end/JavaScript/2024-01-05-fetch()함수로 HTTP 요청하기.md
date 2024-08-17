---
title: "[JS] fetch()함수로 HTTP 요청하기"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

[[프로미스 객체와 메서드↗️]](https://mynamesieun.github.io/javascript/%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4-%EA%B0%9D%EC%B2%B4%EC%99%80-%EB%A9%94%EC%84%9C%EB%93%9C/) 에 이어 작성하는 글입니다.
{: .notice--danger}

# 1. fetch 개요

## 1.1 fetch 개념

> fetch는 HTTP 요청을 보내기 위해 사용하는 내장 API로 별도의 라이브러리 설치 없이 사용할 수 있다.

<span style="color:CornflowerBlue">프로미스(Promise)를 기반</span>으로 설계되어 있어, 비동기 작업을 쉽게 처리할 수 있다. 또한 항상 **프로미스 객체**(resolve, reject)를 반환한다.

<br>

## 1.2 fetch 사용 방법

> fetch 자체는 비동기 방식으로 작동하지만, `.then()`을 통해 마치 동기적으로 처리하는 것처럼 코드를 작성할 수 있다.

```js
fetch(url, options)
  .then((response) => console.log("response:", response))
  .catch((error) => console.log("error:", error));
```

① `fetch()` 함수는 첫 번째 인자로 URL을, 두번째 인자로 옵션 객체(선택, method나 header 등 지정 가능)를 받는다.

② options 객체에는 HTTP 방식(method), HTTP 요청 헤더(headers), HTTP 요청 전문(body) 등을 설정해줄 수 있다.

③ `fetch()`를 호출하면 브라우저는 네트워크 요청을 보내고 Promise 타입의 객체를 반환한다.

④ API 호출이 성공했을 때는 response(응답) 객체를 resolve하고, 실패했을 때는 error(예외) 객체를 reject 한다.

<br>

## 1.3 fetch 단점

> fetch는 아래와 같은 문제가 있다.

1. ⭐예외 처리의 문제: <span style="color:indianred">네트워크 오류 시에만 예외 발생</span>시키고 HTTP 오류에 대해선 예외 발생시키지 않는다.
2. 프로미스 체이닝의 가독성 문제: `.then()`과 `.catch()`로 구성된 프로미스 체이닝이 길어지면 코드의 가독성이 떨어짐
3. 어떤 HTTP 메서드를 사용하는지 불명확
4. 자동 JSON 변환 없음

➡️ [[aixos↗️]](https://mynamesieun.github.io/react/%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-axios%EC%99%80-interceptor/)를 사용하면 fetch의 단점을 해결할 수 있다.

<br><br>

# 2. HTTP 요청하기

| 메서드 |             역할             |
| :----: | :--------------------------: |
|  GET   |     존재하는 자원을 요청     |
|  POST  |    새로운 자원 생성 요청     |
|  PUT   |   존재하는 자원 변경 요청    |
| PATCH  | 존재하는 자원 일부 변경 요청 |
| DELETE |   존재하는 자원 삭제 요청    |

## 2.1 GET 요청

> 존재하는 자원을 요청하며, 단순히 원격 API에 있는 데이터를 가져올 때 사용한다.

① `fetch()` 함수는 디폴트로 GET 방식으로 작동하고, GET 방식은 요청 전문을 받지 않기 때문에 옵션 인자가 필요가 없다.

```js
// posts의 id 1인 엘리먼트를 가져온다.
fetch("https://jsonplaceholder.typicode.com/posts/1").then((response) =>
  console.log(response)
);
```

![](/assets/images/2024/2024-01-05-20-02-52.png)

<br>

② 응답(response) 객체는 `json()` 메서드를 제공하고, 이 메서드를 호출하면 응답(response) 객체로부터 JSON 형태의 데이터를 자바스크립트 객체로 변환하여 얻을 수 있다. [[참고자료↗️]](https://www.daleseo.com/js-window-fetch/)

```js
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

![](/assets/images/2024/2024-01-05-19-49-25.png)

→ 단순히 특정 API에 저장된 데이터를 보여줄때는 GET 방식의 HTTP 통신으로 충분할 것이다.

<br>

## 2.2 POST 요청

> 새로운 자원을 생성을 요청한다.

① 폼 등을 사용해서 데이터를 만들어 낼 때, 보내는 데이터의 양이 많거나, 비밀번호 등 개인정보를 보낼 때 POST 메서드를 사용한다.

② 새로운 포스트 생성을 위해서 method 옵션을 POST로 지정해주고, headers 옵션을 통해 JSON 포맷을 사용한다고 알려줘야 한다.

③ body 옵션에는 요청 전문을 JSON 포맷으로 설정해준다.

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

![](/assets/images/2024/2024-01-05-20-15-30.png)

<br>

## 2.3 PUT 요청

> 존재하는 자원 변경을 요청한다.

① API에서 관리하는 데이터의 수정을 위해 PUT 메서드를 사용한다.

② method 옵션만 PUT으로 설정한다는 점 빼놓고는 POST 방식과 비슷하다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PUT",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    // title 엘리먼트로 전체 데이터를 바꾼다. (마치 innerHTML 같다.)
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

![](/assets/images/2024/2024-01-05-20-19-07.png)

<br>

## 2.4 PATCH 요청

> 존재하는 자원의 일부 변경을 요청한다.

```js
// posts의 id 1인 엘리먼트를 수정한다.
fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PATCH",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    // title만 바꾸고 나머지 요소는 건들지 않는다.
    title: "Test",
  }),
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

![](/assets/images/2024/2024-01-05-21-04-32.png)

<br>

## 2.5 DELETE 요청

> 존재하는 자원 삭제를 요청한다.

DELETE 방식에서는 보낼 데이터가 없기 때문에, headers와 body 옵션이 필요가 없다.

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "DELETE",
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

![](/assets/images/2024/2024-01-05-20-20-07.png)

<br><br>

# 3. 사용성 개선

## 3.1 async / await

① fetch의 리턴값 response 는 Promise 객체이다.

② 따라서 await / async 문법으로 가독성높게 코딩할 수 있다.

```js
fetch("https://jsonplaceholder.typicode.com/posts", option)
  .then((res) => res.text())
  .then((text) => console.log(text));

// await은 async 함수내에서만 쓸수 있으니, 익명 async 바로 실행함수를 통해 활용한다.
(async () => {
  let res = await fetch("https://jsonplaceholder.typicode.com/posts", option);
  let text = res.text();
  console.log(text);
})();
```

<br>

## 3.2 fetch 모듈화

① fetch() 함수는 사용법이 아주 간단하지만, 계속 사용하다보면 똑같은 코드가 반복된다.

② 예를 들어, 응답 데이터을 얻기 위해서 `response.json()`을 매번 호출하거나, 데이터를 보낼 때, HTTP 요청 헤더에 `"Content-Type": "application/json"`로 설정해야한다.

③ 한 두번 fetch() 를 사용하는 정도는 문제가 안되지만, 여러번 사용할 일이 있으면 따로 헬퍼 함수를 만들어 모듈화를 해놓는게 나중에 편하다.

④ 아래 코드와 같이 POST 요청을 보낼 필요가 있다면 함수로 따로 미리 구성을 짜놓고, 인자로 값들을 전달해 바로 응답값을 받는 식으로 구성을 짜놓는 것이 좋다.

```js
async function post(host, path, body, headers = {}) {
  const url = `https://${host}/${path}`;
  const options = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      ...headers,
    },
    body: JSON.stringify(body),
  };
  const res = await fetch(url, options);
  const data = await res.json();
  if (res.ok) {
    return data;
  } else {
    throw Error(data);
  }
}

post("jsonplaceholder.typicode.com", "posts", {
  title: "Test",
  body: "I am testing!",
  userId: 1,
})
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

<br><br>

# 4. 참조

1. [자바스크립트의 fetch() 함수로 원격 API 호출하기](https://www.daleseo.com/js-window-fetch/)
2. [[JavaScript] fetch 함수 쓰는 법, fetch 함수로 HTTP 요청하는 법](https://velog.io/@eunjin/JavaScript-fetch-%ED%95%A8%EC%88%98-%EC%93%B0%EB%8A%94-%EB%B2%95-fetch-%ED%95%A8%EC%88%98%EB%A1%9C-HTTP-%EC%9A%94%EC%B2%AD%ED%95%98%EB%8A%94-%EB%B2%95#24-delete-%EC%A1%B4%EC%9E%AC%ED%95%98%EB%8A%94-%EC%9E%90%EC%9B%90-%EC%82%AD%EC%A0%9C-%EC%9A%94%EC%B2%AD)
3. [[Javascript] fetch() 함수로 원격 API 호출하기](https://frogand.tistory.com/207)
