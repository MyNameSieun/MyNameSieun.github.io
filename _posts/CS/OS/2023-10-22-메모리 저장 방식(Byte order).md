---
title: "[OS] 메모리 저장 방식(Byte order)"
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

# ▶ Byte order

- Byte order
  - 글을 읽을 때 왼쪽에서 오른쪽으로 읽는 것 처럼 컴퓨터도 데이터를 읽을 때의 규칙이 필요하다.
  - 바이트 저장 순서 규칙은 Big-endia와 Little-endian가 있다.
  - Big-endia와 Little-endian를 서로 변환, 해석할 해석기가 필요하다.
    ![](https://velog.velcdn.com/images/sieunpark/post/a9f6e3c3-844f-41c2-bbce-cabe7e24e155/image.jpg)

<br>

## ▷ Big-endian

- Big-endian
  - 최상위 바이트를 먼저 저장한다.
  - 즉, 사람이 숫자를 쓰는 방식과 같이 낮은 주소에 높은 바이트부터 저장한다.
    ![](https://velog.velcdn.com/images/sieunpark/post/e7489d6f-12d9-4293-81e6-b09adea1eb3c/image.png)

<br>

## ▷ Little-endian

- Little-endian
  - 최하위 바이트를 먼저 저장한다.
  - 즉, 낮은 주소에 낮은 바이트부터 저장한다.
    ![](https://velog.velcdn.com/images/sieunpark/post/c6b6722f-d496-49d9-af3f-50a061796265/image.png)

<br>

---

<br>

# 📎참조

- 성결대학교 강영명 교수님 운영체제 (2023)
- https://gdnn.tistory.com/277
