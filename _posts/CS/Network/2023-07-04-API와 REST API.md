---
title: "[Network] API와 REST API"
categories: [Network]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. API 개요

## 1.1 API 개념

> API(Application Programming lnterface)는 두 SW가 요청과 응답을 통해 데이터를 주고받는 매커니즘을 말한다.

- 예를들어 날씨 서비스의 경우, 날씨 API를 사용하여 해당 서비스에 날씨 데이터를 요청하면 서비스는 요청에 대한 응답으로 날씨 데이터를 제공한다. 이를 통해 다른 프로그램에서도 날씨 데이터를 활용할 수 있다.
- API는 클라이언트의 요청에 따라 동적으로 데이터나 정보를 제공한다.
- 주로 JSON이나 XML형식으로 응답을 반환한다.

<br>

## 1.1 API 사용 방법

> API를 사용하려면, API를 호스팅하는 서버의 주소를 알아내고, HTTP를 통해 해당 서버에 요청을 보내면 된다.

① API를 호스팅하는 서버의 주소를 알아내기

- API를 요청하기 위해서는 해당 API를 호스팅하고 있는 주소를 알아야 한다.
- 서버 주소는 IP(Internet Protocol) 주소로 표현되며, "127.012.0.3"과 같은 숫자로 표현되기 때문에 사람이 식별하기 힘들다.
- 따라서 도메인(Domain Name System)을 사용하여 (예: "127.012.0.3"과 같은) ip주소를 (www.text.com과 같은) 문자열로 변환하여 사람들이 쉽게 기억할 수 있도록 해석하는 것이다.
- 도메인은 DNS을 통해 해당 도메인에 대응하는 IP 주소로 해석되어 서버에 접근할 수 있게 된다.
  (IP 주소가 실제로는 서버 위치를 가리키는 것이다.)

<br>

② 서버에 API를 요청하기 위해서는 HTTP 프로토콜을 사용한다.

<br>

③ HTTP 요청 메시지에는 요청의 목적과 세부 정보가 포함된다.

- HTTP 요청 메서드(GET, POST, PUT, DELETE 등)를 지정하여 클라이언트가 서버에게 원하는 동작을 전달한다.

<br>

④ 서버는 클라이언트의 요청을 받아 처리하고, HTTP 응답 메시지를 생성하여 클라이언트에게 반환한다.

- 요청에 대한 처리 결과를 HTTP 응답 메시지에 담아 클라이언트에게 전송한다.
- 응답 메시지에는 상태 코드(ex: 2xx 성공, 4xx 클라이언트 오류)와 함께 요청에 대한 결과 또는 오류 정보가 포함된다.

<br>

⑤ 클라이언트는 서버로부터 받은 응답을 처리한다.

- 클라이언트는 HTTP 응답 메시지를 받아서 요청한 작업에 따른 데이터를 활용한다.

<br><br>

# 2. REST API 개요

## 2.1 REST 개념

> REST(REpresentational State Transfe)는 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.

즉,

- HTTP URI를 통해 자원(Resource)을 명시하고,
- 어떤 자원에 대해 CRUD를 진행할 수 있게
- HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 사용하여
- 해당 자원(URI)에 대한 요청을 보내는 것을 말한다.

<br>

## 2.2 REST 구성 요소

- 자원(Resource): HTTP URI

  - 자원을 구별하는 ID는 ‘/groups/:group_id’와 같은 HTTP URI 다.
  - Client는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 Server에 요청한다.

   <br>

- 자원에 대한 행위(Verb): HTTP Method

  - HTTP 프로토콜의 Method를 사용한다.
  - HTTP 프로토콜은 GET, POST, PUT, DELETE 와 같은 메서드를 제공한다.

   <br>

- 자원에 대한 행위의 내용(표현): HTTP Message Pay Load
  - Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
  - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있다.
  - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.<br>
    출처: (https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

<br>

## 2.3 REST API 개념

> REST를 기반으로 만들어진 API를 REST API라고 한다.

REST를 사용하지 않는 API는 일반적으로 HTTP 요청 메서드를 사용하지 않고 아래와 같이 CREATE, READ, UPDATE와 같은 개선되지 않은 CRUD 동작을 수행한다.

REST를 사용하지 않고 영화 API를 생성해보자

```jsx
/createMovie
/seeMovies
/getMovie/inception
/deleteMovie/inception
/updateMovie/inception
/getTopRatedMovies
/findMoviesFromThisYear
```

위와 같은 설계는 명확한 패턴이 없기 때문에 유저와 팀원들은 이해하기 어려울 수 있다.

<br>

> 따라서 위와같은 문제를 해결하기 위해 REST가 필요한 것이다.

| CRUD Operation |       HTTP Method       |
| :------------: | :---------------------: |
|     Create     |    데이터 생성(POST)    |
|      Read      |    데이터 조회(GET)     |
|     Update     | 데이터 수정(PUT, PATCH) |
|     Delete     |   데이터 삭제(DELETE)   |

```jsx
GET /movies: 모든 영화를 가져오는 요청
POST /movies: 새로운 영화를 등록하는 요청
GET /movies/{id}: 특정 영화를 가져오는 요청 (예: /movies/inception)
PUT /movies/{id}: 특정 영화를 수정하는 요청 (예: /movies/inception)
DELETE /movies/{id}: 특정 영화를 삭제하는 요청 (예: /movies/inception)
GET /topRatedMovies: 최상위 평점을 받은 영화를 가져오는 요청
```

위와같이 RESTful하게 만든 API는 요청을 보내는 주소 만으로도 대략 어떤 동작을 하는 동작인지 파악이 가능하다.

<br>

## 2.4 REST API 규칙

```jsx
http://example.com/posts     (O)
http://example.com/posts/    (X)
http://example.com/post      (X)
http://example.com/get-posts (X)
--> URI는 명사를 사용하고 소문자로 작성되어야 한다.
--> 명사는 복수형을 사용한다.
--> URI의 마지막에는 /를 포함하지 않는다.

http://example.com/post-list  (O)
http://example.com/post_list  (X)
--> URI에는 언더바가 아닌 하이픈을 사용한다.

http://example.com/post/assets/example  (O)
http://example.com/post/assets/example.png  (X)
--> URI에는 파일의 확장자를 표시하지 않는다.
```

<br>

## 2.5 RESTful API

> REST API의 조건을 만족시킨 통신 설계 상태를 말한다.

즉, 누구나 이해하기 쉽고 사용하기 쉬운 REST API를 보고 "RESTful 하다." 라고 말하는 것이다.

<br>

아래의 경우 RESTful하지 못한 경우이다.

- CRUD의 기능을 모두 POST method로만 처리한 API
- URL에 resource, id외의 정보인 행위(method)에 대한 부분이 들어가는 경우(/classes/createPeople)

<br><br>

# 3. Path Variable과 Query Parameter

## 3.1 Path Variable

경로를 변수로서 사용하는 방법이다.

```js
/users/10
```

전체 데이터 또는 하나의 데이터를 다룰 때처럼 특정 resource를 식별하고 싶을 때 사용한다.

<br>

## 3.2 Query Parameter

```js
/users?user_id=10
```

데이터를 정렬이나 필터링하는 경우 사용한다.

<br><br>

# 4. URL

> 서버에 자원을 요청하기 위해 입력하는 영문 주소

- URL을 통해 웹 브라우저에서 접속하고자 하는 웹 페이지의 주소를 입력하면, HTTP 프로토콜을 사용하여 이 주소에 해당하는 웹 서버에 요청(Request)을 보낸다.
- URL의 구조는 아래와 같다.<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/48905101-1538-4f96-8495-64cb68227824/image.png)

<br><br>

# 5. 참조

- https://velog.io/@hyo123/HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CAPI
- https://joshua1988.github.io/web-development/http-part1/
- https://khj93.tistory.com/m/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80

<br>
