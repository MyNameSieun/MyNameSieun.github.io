---
title: "[CSS] Box-sizing"
categories: [CSS]
tag: [CSS]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ Box-sizing

- box-sizing 속성은 요소의 너비와 높이를 계산하는 방법을 지정한다.
- 기본값인 content-box는 콘텐츠 영역만을 고려하고, border-box는 콘텐츠 영역에 안쪽 여백과 테두리를 포함한 전체 크기를 고려한다.

<br>

보통 아래와 같이 CSS에서 미리 선언하고 시작한다.

```jsx
* {
  box-sizing: border-box;
}
```

<br>

## ▷ content-box

> 기본값, 콘텐츠 영역의 크기에만 영향을 받고 안쪽 여백, 테두리, 바깥 여백은 추가 공간을 차지하지 않는다.

```jsx
// index.html
    <div>box 1</div>
    <div>box 2</div>

```

```jsx
// style.css
div {
  width: 150px;
  margin: 10px;
  box-sizing: content-box; //기본값
}

div {
  border: 1px solid;
  margin-top: 10px;
}

div:nth-of-type(2) {
  border: solid 30px;
}
```

![](https://velog.velcdn.com/images/sieunpark/post/d20e491d-57dd-494e-9482-c38ef001f49e/image.png)

<br>

## ▷ border-box

> 요소의 너비와 높이에 콘텐츠 영역, 안쪽 여백, 테두리가 포함된다.
> 콘텐츠 영역에 더 많은 내용이 들어가더라도 요소의 전체 크기는 변하지 않지만, 요소의 내부 공간이 축소될 수 있다.

```jsx
// index.html
    <div>box 1</div>
    <div>box 2</div>

```

```jsx
// style.css
div {
  width: 150px;
  margin: 10px;
  box-sizing: content-box; //기본값
}

div {
  border: 1px solid;
  margin-top: 10px;
}

div:nth-of-type(2) {
  border: solid 30px;
}
```

![](https://velog.velcdn.com/images/sieunpark/post/9b294eab-3bfd-4f4d-a096-3475d5e19d3a/image.png)

<br>

---

<br>

# 📎참조

https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing
