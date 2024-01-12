---
title: "[OS] Microkernels"
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

# ▶ Microkernels

- 특징
  - 커널이 커져 관리가 어려워지면서 가능한 많은 기능을 커널에서 사용자 공간으로 옮겨 커널을 최소화하는 설계 방식이다. ( 커널에서 최소한의 기능만을 제공 )
  - Mach : 카네기 멜론 대학교에서 개발한 첫 번째 **Microkernels**이다.
  - Mac OS X 커널(Darwin)은 부분적으로 <span style="color:indianred">Mach</span>에 기초한다.
  - 사용자 모듈 간의 통신은 <span style="color:indianred">message passing</span>을 사용한다.
    - [massage passing에 대해 더 자세히 알고싶으면 클릭!](https://velog.io/@sieunpark/OS-IPC-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EA%B0%84-%ED%86%B5%EC%8B%A0)

<br>

- 장점
  - 모듈화 : 각 서비스는 독립적으로 작동하므로 모듈화가 쉬워 확장하기 좋다.
  - 유연성 : 새로운 H/W, S/W 에 이식이 쉽다.
  - 안정성 : 한 서비스에 문제가 생겨도도 그 영향력이 해당 모듈에 국한되므로 전체 시스템에 대한 영향을 최소화할 수있어 보안 위협에 대한 리스크를 줄일 수 있다.
  - 신뢰성, 보안성 : 커널 모드가 작기 때문에 신뢰성, 보안성이 향상된다.

<br>

- 단점
  - 오버헤드 : 사용자와 커널 간의 통신으로 인한 성능 오버헤드 발생
  - 복잡성 : 사용자 공간에서 많은 서비스를 처리해야 하기 때문에 설계 및 구현이 복잡해질 수 있음
    ![](https://velog.velcdn.com/images/sieunpark/post/2acc4ab9-7383-4fd2-add6-2f8832cd033c/image.png)

<br>

---

<br>

# 📎참조

- 성결대학교 강영명 교수님 운영체제 (2023)
- https://taegyunwoo.github.io/os/OS_Structure
