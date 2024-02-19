---
title: "[Network] HTTP / HTTPS"
categories: [Network]
tag: [Network, CS]
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

- HTTP는 인터넷에서 데이터를 주고받기 위한 통신 프로토콜이다.
- 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 보낸다.<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/f8d67d91-bbff-46c1-8099-d0f584bd5d8c/image.png)

<br>

## 1.2 HTTP 요청 메서드

- 클라이언트는 HTTP 요청 메세지를 생성하고, 해당 서버에 해당 서버의 주소(URL)와 함께 요청을 전송한다고 하였다.
- 즉, URL을 이용하면 서버에 특정 데이터를 요청할 수 있는 것이다.

| 요청 메서드 |                                                  설명                                                  |
| :---------: | :----------------------------------------------------------------------------------------------------: |
|     GET     |               서버 자원을 가져오고자 할 때 사용, 요청의 본문(body)에 데이터를 넣지 않음                |
|    POST     |          서버에 자원을 새로 등록하고자 할 때 사용, 청의 본문에 새로 등록할 데이터를 넣어 보냄          |
|     PUT     | 서버의 자원을 요청에 들어 있는 자원으로 치환하고자 할 때 사용, 요청의 본문에 치환할 데이터를 넣어 보냄 |
|   DELETE    |                 서버의 자원을 삭제하고자 할 때 사용, 요청의 본문에 데이터를 넣지 않음                  |

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

# 2.2 HTTPS 보안 방식

> ① SSL/TLS

HTTPS 프로토콜은<span style="color:indianred"> SSL/TLS 인증서를 통해 데이터를 암호화</span>하여 <span style="color:indianred">보안을 강화</span>한다!

SSL/TLS은 암호화 기반 인터넷 보안 프로토콜을 의미하며, 웹 사이트와 브라우저 사이 전송되는 데이터를 암호화하여 민감한 정보를 주고받을 때 데이터를 도난 당하는 것을 보호해준다.

SSL/TLS은 데이터 무결성을 제공하기 때문에, 데이터가 전송 중에 수정되거나 손상되는 것을 방지하고 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증하는 인증기능을 제공해준다.

<br>

- SSL(보안 소켓 계층)에서 개인정보 보호 및 보안 기능이 추가된 암호화 프로토콜을 TLS(전송 계층 보안)이라고 부른다.
- 둘은 기능과 용도가 같기 때문에 SSL/TLS 로 붙여서 사용하기도 한다.

<br>

> ② 대칭키/공개키

SSL/TLS은 데이터를 암호화하기 위해 대칭키 암호화와 공개키(비대칭키) 암호화 방법을 혼용해서 사용한다.

SSL/TLS에 관한 암호화 방식에 대해선 https://babbab2.tistory.com/4 여기서 확인해보자
(추후 공개키 암호와 대칭키 암호에 대해 포스팅할 예정)

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
  출처: https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/

<br>

## 3.2 비연결성 (Connectionless)

- 서버와 클라이언트는 연결되어 있지 않는다. 서버 입장에서는 매번 새로운 요청이다.
- 비연결성으로 인해 최소한의 서버 자원으로 서버를 유지할 수 있게 해준다.
- 그러나, 각 사용자별 요청이 잦은 서비스의 경우 이러한 비연결성은 오히려 비효율적일 수 있다.

![](/assets/images/2024/2024-02-19-16-12-30.png)
출처: https://hanamon.kr/네트워크-http-http란-특징-무상태-비연결성/

<br><br>

# 4. 참조

- https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/
- https://brunch.co.kr/@hyoi0303/10
- https://seo.tbwakorea.com/blog/https-http/
- https://www.youtube.com/watch?v=gbTknWju8H4&t=48s
- https://n-log.tistory.com/29
