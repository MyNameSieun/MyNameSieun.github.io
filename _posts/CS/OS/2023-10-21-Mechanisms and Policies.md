---
title: "[OS] Mechanisms and Policies"
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

# ▶ Mechanisms and Policies

- 중요한 원칙
  - 정책과 메커니즘을 분리를 해야한다.
  - 나중에 정책이 변경될 때 최대한의 융통성이 생기기 때문이다.
    - 정책은 장소가 바뀌거나 시간의 흐름에 따라 변경될 수 있다.
    - 정책에 민감하지 않은 기법이 바람직하다.

<br>

- 정책과 기법
  - 정책(Policies): <span style="color:indianred">무엇을 (규칙)</span> 할 것인가?
  - 기법(Mechanism): <span style="color:indianred">어떻게 (도구)</span> 할 것인가?

<br>

- 예시
  - CPU를 계속 점유하는 것을 방지하기 위해 Timer를 사용한다. ( 기법 )
  - 특정 사용자에게 Timer를 얼마나 오랫동안 설정할 것인지 결정한다. ( 정책 )

<br>

---

<br>

# 📎참조

- 성결대학교 강영명 교수님 운영체제 (2023)
