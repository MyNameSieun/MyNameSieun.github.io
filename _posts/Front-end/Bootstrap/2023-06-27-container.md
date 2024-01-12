---
title: "[Bootstrap] container"
categories: [Bootstrap]
tag: [Bootstrap]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ container란?

- 컨테이너는 부트스트랩에서 가장 기본적인 레이아웃 요소이다.
- 다양한 기기에서 그리드 열의 너비가 자동으로 조절되어 반응형 웹을 만들 수 있다.
- 컨테이너는 페이지의 중앙에 가로 정렬된다.
  ![](https://velog.velcdn.com/images/sieunpark/post/69298e9e-9f78-469e-86fb-6f7d8d0a6716/image.png)

> 👶🏻 언니, 컨테이너가 뭐야?
> 👩🏻‍💻컨테이너는 부트스트랩에서 가장 기본적인 레이아웃 요소야.
> 컨테이너는 웹 페이지의 내용을 감싸는 박스로, 그 안에 콘텐츠를 배치하는데 사용되곤 해.
> 부트스트랩에서 컨테이너의 클래스(class)를 사용하여 반응형 웹을 만들 수 있어

<br>

## ▷ container

> 고정 너비를 가진 컨테이너를 생성한다.

```
<div class="container">
  <p>행복한 하루 보내세용 (❁´◡`❁)</p>
</div>
```

 <br>

## ▷container-fluid

> 전체 너비를 가진 컨테이너를 생성한다.
> 가로 해상도에 상관없이 항상 width가 100%의 값을 갖는다.

```
    <h1 class="container-fluid bg-primary">container-fluid 적용0</h1>
    <h1 class="container bg-danger-subtle">container-fluid 적용x</h1>
```

![](https://velog.velcdn.com/images/sieunpark/post/a06af7cb-810d-40c3-941d-548150929ed7/image.gif)

<br>

---

<br>

# 📎참조

https://getbootstrap.com/docs/5.3/layout/containers/#how-they-work
