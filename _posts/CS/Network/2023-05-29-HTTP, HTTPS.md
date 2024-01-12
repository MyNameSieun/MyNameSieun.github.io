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

지난 [API와 REST API](https://velog.io/@sieunpark/OS-API%EC%99%80-REST-API)를 포스팅 했을 때 HTTP가 무엇인지를 살펴보았다.

이번 포스팅에서는 HTTP와 HTTPS의 차이를 살펴보자!

<br>

---

<br>

# ▶ HTTP와 HTTPS의 차이

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

---

<br>

# ▶ HTTPS 보안 방식

## ▷ SSL/TLS

> HTTPS 프로토콜은<span style="color:indianred"> SSL/TLS 인증서를 통해 데이터를 암호화</span>하여 <span style="color:indianred">보안을 강화</span>한다!

SSL/TLS은 암호화 기반 인터넷 보안 프로토콜을 의미하며, 웹 사이트와 브라우저 사이 전송되는 데이터를 암호화하여 민감한 정보를 주고받을 때 데이터를 도난 당하는 것을 보호해준다.

<br>

- SSL(보안 소켓 계층)에서 개인정보 보호 및 보안 기능이 추가된 암호화 프로토콜을 TLS(전송 계층 보안)이라고 부른다.
- 둘은 기능과 용도가 같기 때문에 SSL/TLS 로 붙여서 사용하기도 한다.

<br>

## ▷ 대칭키/공개키

> SSL/TLS은 데이터를 암호화하기 위해 대칭키 암호화와 공개키(비대칭키) 암호화 방법을 혼용해서 사용한다.

SSL/TLS에 관한 암호화 방식에 대해선 https://babbab2.tistory.com/4 여기서 확인해보자
(추후 공개키 암호와 대칭키 암호에 대해 포스팅할 예정)

<br>

> SSL/TLS은 데이터 무결성을 제공한다.

따라서 데이터가 전송 중에 수정되거나 손상되는 것을 방지하고 사용자가 자신이 의도하는 웹사이트와 통신하고 있음을 입증하는 인증기능을 제공해준다.

<br>

- HTTP - TCP (HTTP는 바로 TCP와 통신)

- HTTPS - SSL/TLS - TCP (HTTPS는 SSL을 거쳐 TCP와 통신)

<br>

---

<br>

# 📎참조

- https://aws.amazon.com/ko/compare/the-difference-between-https-and-http/
- https://brunch.co.kr/@hyoi0303/10
- https://seo.tbwakorea.com/blog/https-http/
- https://www.youtube.com/watch?v=gbTknWju8H4&t=48s
- https://n-log.tistory.com/29
