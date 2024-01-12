---
title: "[OS] API와 REST API (RESTful 작성해야함)"
categories: [OS]
tag: [OS, CS]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ API

> API(Application Programming lnterface)는 두 SW가 요청과 응답을 통해 데이터를 주고받는 매커니즘을 말한다.

- 예를들어 날씨 서비스의 경우, 날씨 API를 사용하여 해당 서비스에 날씨 데이터를 요청하면 서비스는 요청에 대한 응답으로 날씨 데이터를 제공한다. 이를 통해 다른 프로그램에서도 날씨 데이터를 활용할 수 있다.

<br>

## ▷ API 사용 방법

> API를 사용하기 위해선 어떻게 하면 될까?
> -> API를 호스팅하는 서버의 주소를 알아내고, HTTP를 통해 해당 서버에 요청을 보내면 된다.

> 1.  API를 호스팅하는 서버의 주소를 알아내기

- API를 요청하기 위해서는 해당 API를 호스팅하고 있는 주소를 알아야 한다.
- 서버 주소는 IP(Internet Protocol) 주소로 표현되며, "127.012.0.3"과 같은 숫자로 표현되기 때문에 사람이 식별하기 힘들다.
- 따라서 도메인(Domain Name System)을 사용하여 ex) ("127.012.0.3"과 같은) ip주소를 (www.text.com과 같은) 문자열로 변환하여 사람들이 쉽게 기억할 수 있도록 해석하는 것이다.
- 도메인은 DNS을 통해 해당 도메인에 대응하는 IP 주소로 해석되어 서버에 접근할 수 있게 된다.
  (IP 주소가 실제로는 서버 위치를 가리키는 것이다.)

> 2. 서버에 API를 요청하기 위해서는 HTTP [프로토콜](https://velog.io/@sieunpark/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)을 사용한다.

- HTTP는 인터넷에서 데이터를 주고받기 위한 통신 프로토콜이다.
- HTTP 프로토콜을 사용하여 API를 호스팅하는 서버에 요청을 보낸다.
- 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 전송한다.
  ![](https://velog.velcdn.com/images/sieunpark/post/f8d67d91-bbff-46c1-8099-d0f584bd5d8c/image.png)

> 3. HTTP 요청 메시지에는 요청의 목적과 세부 정보가 포함된다.

- HTTP 요청 메서드(GET, POST, PUT, DELETE 등)를 지정하여 클라이언트가 서버에게 원하는 동작을 전달한다.

> 4. 서버는 클라이언트의 요청을 받아 처리하고, HTTP 응답 메시지를 생성하여 클라이언트에게 반환한다.

- 요청에 대한 처리 결과를 HTTP 응답 메시지에 담아 클라이언트에게 전송한다.
- 응답 메시지에는 상태 코드(예: 200 OK, 404 Not Found)와 함께 요청에 대한 결과 또는 오류 정보가 포함된다.

> 5.  클라이언트는 서버로부터 받은 응답을 처리한다.

- 클라이언트는 HTTP 응답 메시지를 받아서 요청한 작업에 따른 데이터를 활용한다.

<br>

---

<br>

# ▶ HTTP 요청 메서드

> - 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 전송한다고 하였다.

- 즉, URL을 이용하면 서버에 특정 데이터를 요청할 수 있는 것이다.

| 요청 메서드 |                                                  설명                                                  |
| :---------: | :----------------------------------------------------------------------------------------------------: |
|     GET     |               서버 자원을 가져오고자 할 때 사용, 요청의 본문(body)에 데이터를 넣지 않음                |
|    POST     |          서버에 자원을 새로 등록하고자 할 때 사용, 청의 본문에 새로 등록할 데이터를 넣어 보냄          |
|     PUT     | 서버의 자원을 요청에 들어 있는 자원으로 치환하고자 할 때 사용, 요청의 본문에 치환할 데이터를 넣어 보냄 |
|   DELETE    |                 서버의 자원을 삭제하고자 할 때 사용, 요청의 본문에 데이터를 넣지 않음                  |

<br>

---

<br>

# ▶ REST API

> REST를 기반으로 만들어진 API를 REST API라고 한다.

> REST를 사용하지 않는 API는 일반적으로 HTTP 요청 메서드를 사용하지 않고 아래와 같이 CREATE, READ, UPDATE와 같은 개선되지 않은 CRUD 동작을 수행한다.

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
그렇다면 REST와 RESTful이 뭔지 알아보자

<br>

## ▷ REST

> REST는 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다.

즉,

- HTTP URI를 통해 자원(Resource)을 명시하고,
- HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해
- 해당 자원(URI)에 대한 CRUD를 적용하는 것을 의미한다.

<br>

## ▷ REST 구성 요소

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
  - JSON 혹은 XML를 통해 데이터를 주고 받는 것이 일반적이다.

(https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html 에 있는 글을 가져왔다.)

<br>

## ▷ REST의 특징

1. Server-Client(서버-클라이언트 구조)
   REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됩니다.

2. Stateless(무상태성)
   REST는 무상태성 성격을 갖습니다. 다시 말해 작업을 위한 상태정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.

3. Cacheable(캐시 처리 가능)
   REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능합니다. 따라서 HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능합니다.
4. 계층형 구조
   REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 합니다.

5. Self-descriptiveness (자체 표현 구조)
   REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것입니다.

(https://meetup.nhncloud.com/posts/92 에 있는 글을 가져왔다.)

<br>

---

<br>

# ▶ URL

> 서버에 자원을 요청하기 위해 입력하는 영문 주소

- URL을 통해 웹 브라우저에서 접속하고자 하는 웹 페이지의 주소를 입력하면, HTTP 프로토콜을 사용하여 이 주소에 해당하는 웹 서버에 요청(Request)을 보낸다.
- URL의 구조는 아래와 같다.
  ![](https://velog.velcdn.com/images/sieunpark/post/48905101-1538-4f96-8495-64cb68227824/image.png)

<br>

---

<br>

# 📎참조

- https://velog.io/@hyo123/HTTP-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9CAPI
- https://joshua1988.github.io/web-development/http-part1/
- https://khj93.tistory.com/m/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80
