---
title: "[Network] HTTP / HTTPS"
categories: [Network]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. HTTP 개요

## 1. HTTP 개념

- HTTP는 인터넷에서 데이터를 주고받기 위한 <span style="color:indianred">통신 프로토콜</span>이다.
- 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 보낸다.<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/f8d67d91-bbff-46c1-8099-d0f584bd5d8c/image.png)

  <br>

> 서버와 클라이언트의 개념은 상대적이다!

아래 사진에서 웹브라우저가 웹서버에 날씨데이터를 요청하고, 웹서버는 다시 기상청 서버에 데이터를 요청 후 리턴을 받아 그 결과를 웹브라우저에게 전달한다.

이때 웹서버는 서버의 역할도 하지만, 데이터를 요청하는 클라이언트의 역할도 한다.

![](/assets/images/2024/2024-02-25-23-07-52.png)

  <br>

## 1.2 HTTP 요청 메서드

- 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 전송한다고 하였다.
- 즉, URL을 이용하면 서버에 특정 데이터를 요청할 수 있는 것이다.

| 요청 메서드 |                                      설명                                      |
| :---------: | :----------------------------------------------------------------------------: |
|     GET     |             자원 가져오기, 요청의 본문(body)에 데이터를 넣지 않음              |
|    POST     |          자원 새로 등록, 요청의 본문에 새로 등록할 데이터를 넣어 보냄          |
|   DELETE    |         자원 삭제, 요청의 본문에 데이터를 넣지 않음 ( URL로 자원 지정)         |
|     PUT     | 자원 전체 업데이트 (본문에 전체 데이터 포함), 자원이 존재하지 않으면 새로 생성 |
|    PATCH    |                  자원 부분 업데이트 (본문에 부분 데이터 포함)                  |

<br>

백엔드에서 아래와 같이 요청에 대한 응답을 정의해 놓는다.

```js
// URL은 같은데 메서드는 다르므로 다른 요청
@Get("/faq/:id")

@Patch("/fqp/:id")
```

<br>

### 1.2.1 GET

같은 예금 창구에서도 개인 고객이냐 기업 고객이냐에 따라 가져와야 하는 것 / 처리해주는 것이 다른 것처럼,
클라이언트가 요청 할 때에도, "타입"이라는 것이 존재한다.

- GET

  - 통상적으로 <span style="color:indianred">데이터 조회(Read)</span>를 요청할 때
  - ex. 영화 목록 조회

- POST
  - 통상적으로 데이터 <span style="color:indianred">생성(Create), 변경(Update), 삭제(Delete)</span> 요청 할 때
  - ex. 회원 가입, 회원 탈퇴, 비밀번호 수정

<br>

> GET method는 클라이언트에서 서버로 어떠한 리소스로 부터 정보를 요청하기 위해 사용되는 메서드이다. 즉, 서버에서 어떤 데이터를 가져와서 보여줄때 사용한다. (값이나 내용, 상태등을 바꾸지 않는 경우에 사용)

- https://movie.daum.net/moviedb/main?movieId=68593 주소는 크게 두 부분으로 쪼개진다. 바로 "?"가 쪼개지는 지점이다.
  "?" 기준으로 앞 부분이 <서버 주소> 뒷 부분이 <영화 번호> 이다.
  - 서버 주소: https://movie.daum.net/moviedb/main?movieId=68593
  - 영화 정보: movieId=68593

<br>

> GET 방식으로 데이터를 전달하는 방법

- ? : 여기서부터 전달할 데이터가 작성된다는 의미이다.
- & : 전달할 데이터가 더 있다는 뜻이다. (요청할 파라미터가 여러개일 때, &로 연결)
  <br>

- e.g., `google.com/search?q=아이폰&sourceid=chrome&ie=UTF-8`
  위 주소는 google.com의 search 창구에 다음 정보를 전달한다.
  - q=아이폰 (검색어)
  - sourceid=chrome (브라우저 정보)
  - ie=UTF-8 (인코딩 정보)

<br>

> 그럼 code라는 이름으로 영화 번호를 주자!는 것은 누가 정하는 것일까?

- 바로 프론트엔드 개발자와 백엔드 개발자가 미리 정해둔 약속이다.
  - 👩🏻‍🦰 프론트엔드: "code라는 이름으로 영화 번호를 주면 될까요?"
  - 👱🏻‍♂️ 백엔드: "네 그렇게 하시죠. 그럼 code로 영화 번호가 들어온다고 생각하고 코딩하고 있을게요"

<br>

그렇다. 우리는 하루에도 수십번씩 GET 방식을 사용하고 있는 것이다❗

<br><br>

# 2. HTTPS

## 2.1 HTTP vs HTTPS

> 우선 차이점을 표로 살펴보자

|               |                              HTTP                              |               HTTPS                |
| :-----------: | :------------------------------------------------------------: | :--------------------------------: |
|     본말      |                  Hypertext Transfer Protocol                   | Hypertext Transfer Protocol Secure |
| 기본 프로토콜 | HTTP/1과 HTTP/2는 TCP/IP를 사용, HTTP/3은 QUIC 프로토콜을 사용 |     SSL/TLS와 함께 HTTP/2 사용     |
|   기본 포트   |                               80                               |                443                 |
|     보안      |                         보안 기능 없음                         | 퍼블릭 키 암호화에 SSL 인증서 사용 |

<br>

> HTTP와 HTTPS의 가장 큰 차이점은 <span style="color:indianred">보안</span>이다.

HTTP [프로토콜](https://velog.io/@sieunpark/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)은 서버에서부터 브라우저로 전송되는 정보가 암호화 되지 않는다. 웹상의 데이터 메세지는 누구나 읽을 수 있는 일반 텍스트로 전송이 되기 때문이다. 따라서 전송되는 데이터가 누구나 볼 수 있어 데이터를 쉽게 도난당할 수 있다는 문제가 있다.

<br>

> SSL 인증서를 HTTP 프로토콜에 추가하여 <span style="color:indianred">보안 문제를 해결한 것이 HTTPS</span>이다.
> ( HTTP + S(Secure) = HTTPS )

HTTPS를 사용하면 비밀번호와 같은 민감한 정보가 암호화되어 전송되기 때문에 누군가가 해당 정보를 가로챈다고 해도 암호화된 상태로 존재하여 해독할 수 없다.

<br>

## 2.2 HTTPS 보안 방식

> ① SSL/TLS

- HTTPS 프로토콜은<span style="color:indianred"> SSL/TLS 인증서를 통해 데이터를 암호화</span>하여 <span style="color:indianred">보안을 강화</span>한다!
- SSL/TLS은 암호화 기반 인터넷 보안 프로토콜을 의미하며, 웹 사이트와 브라우저 사이 전송되는 데이터를 암호화하여 민감한 정보를 주고받을 때 데이터를 도난 당하는 것을 보호해준다.
- SSL/TLS은 데이터 무결성을 제공하기 때문에, 데이터가 전송 중에 수정되거나 손상되는 것을 방지하고 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증하는 인증기능을 제공해준다.

<br>

- SSL(보안 소켓 계층)에서 개인정보 보호 및 보안 기능이 추가된 암호화 프로토콜을 TLS(전송 계층 보안)이라고 부른다.
- 둘은 기능과 용도가 같기 때문에 SSL/TLS 로 붙여서 사용하기도 한다.

<br>

> ② 대칭키/공개키

- SSL/TLS은 데이터를 암호화하기 위해 대칭키 암호화와 공개키(비대칭키) 암호화 방법을 혼용해서 사용한다.
- SSL/TLS에 관한 암호화 방식에 대해선 [[여기↗️]](https://babbab2.tistory.com/4)서 확인해보자 (추후 공개키 암호와 대칭키 암호에 대해 포스팅할 예정)

<br>

- HTTP - TCP (HTTP는 바로 TCP와 통신)
- HTTPS - SSL/TLS - TCP (HTTPS는 SSL을 거쳐 TCP와 통신)

<br><br>

# 3. http 프로토콜 통신의 특징

## 3.1 무상태(Stateless)

- 서버는 클라이언트의 상태를 기억하지 않는다. 따라서 각 요청마다 서버에서 요구하는 모든 상태 정보를 담아서 요청해야 한다.
- 상태값은 매 요청마다 클라이언트가 가지고 오기 때문에, 서버는 클라이언트의 상태를 별도로 기억할 필요없이 주문받은 대로 응답해준다.
- 무상태라는 특성 덕분에 동일한 서버를 여러개로 확장시킬 수 있다. (Scale-Out)

  ![](/assets/images/2024/2024-02-19-16-11-34.png)
  출처: [https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/](https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/)

<br>

## 3.2 비연결성 (Connectionless)

- 서버와 클라이언트는 연결되어 있지 않는다. 서버 입장에서는 매번 새로운 요청이다.
- 비연결성으로 인해 최소한의 서버 자원으로 서버를 유지할 수 있게 해준다.
- 그러나, 각 사용자별 요청이 잦은 서비스의 경우 이러한 비연결성은 오히려 비효율적일 수 있다.

![](/assets/images/2024/2024-02-19-16-12-30.png)
출처: [https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/](https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/)

💡 HTTP의 2가지 특성으로 인해 서버는 클라이언트의 상태를 알 수 없다는 문제가 있다. 따라서 라이언트의 상태를 알기 위해 [[쿠키, 세션, 토큰↗️]](https://mynamesieun.github.io/network/%EC%BF%A0%ED%82%A4,-%EC%84%B8%EC%85%98,-%ED%86%A0%ED%81%B0,-JWT/)을 사용한다.

<br><br>

# 4. 에러 코드

## 4.1 400번대 에러

> 클라이언트 잘못

| HTTP Status Code           | Description                                                                                                  |
| -------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **400 Bad Request**        | 클라이언트의 요청이 잘못되었을 때 발생한다. 예를 들어, 요청 형식이 잘못되었거나 요청이 불완전할 때 발생한다. |
| **401 Unauthorized**       | 요청이 인증을 필요로 할 때 사용되며, 제공된 자격 증명이 없거나 잘못되었을 때 발생한다.                       |
| **403 Forbidden**          | 클라이언트가 자원에 접근할 권한이 없음을 나타낸다. 서버가 요청을 이해했지만 승인을 거부한다.                 |
| **404 Not Found**          | 서버가 요청한 리소스를 찾을 수 없을 때 발생한다. 주로 잘못된 URL로 인해 발생한다.                            |
| **405 Method Not Allowed** | 요청한 리소스에서 허용되지 않는 HTTP 메소드를 사용했을 때 발생한다.                                          |

<br>

## 4.2 500번대 에러

> 서버 잘못 (서버 폭파, 서버 오류 등으로 응답을 줄 수 없는 상태)

| HTTP Status Code              | Description                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| **500 Internal Server Error** | 서버 내부에서 일반적인 오류가 발생했을 때 사용된다. 구체적인 오류 원인이 명시되지 않는다.                                               |
| **501 Not Implemented**       | 서버가 요청된 기능을 지원하지 않을 때 발생한다. 주로 서버가 요청된 메소드를 인식하지 못하는 경우에 발생한다.                            |
| **502 Bad Gateway**           | 서버가 게이트웨이나 프록시로 작동 중일 때 다른 서버로부터 잘못된 응답을 받았을 때 발생한다.                                             |
| **503 Service Unavailable**   | 서버가 일시적으로 요청을 처리할 수 없을 때 발생한다. 보통 서버 과부하나 유지보수로 인해 일시적으로 서비스를 사용할 수 없을 때 사용된다. |
| **504 Gateway Timeout**       | 게이트웨이나 프록시 서버가 다른 서버로부터 시간 내에 응답을 받지 못했을 때 발생한다.                                                    |

<br><br>

# 5. 참조

- [https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/](https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/)
- [https://brunch.co.kr/@hyoi0303/10](https://brunch.co.kr/@hyoi0303/10)
- [https://seo.tbwakorea.com/blog/https-http/](https://seo.tbwakorea.com/blog/https-http/)
- [https://www.youtube.com/watch?v=gbTknWju8H4&t=48s](https://www.youtube.com/watch?v=gbTknWju8H4&t=48s)
- [https://n-log.tistory.com/29](https://n-log.tistory.com/29)
- [https://velog.io/@songyouhyun/Get%EA%B3%BC-Post%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%84%EC%8B%9C%EB%82%98%EC%9A%94](https://velog.io/@songyouhyun/Get%EA%B3%BC-Post%EC%9D%98-%EC%B0%A8%EC%9D%B4%EB%A5%BC-%EC%95%84%EC%8B%9C%EB%82%98%EC%9A%94)

<br>
