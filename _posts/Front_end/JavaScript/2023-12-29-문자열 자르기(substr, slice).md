---
title: "[JS] 문자열 자르기(substr, slice)"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. substr, slice

> substr, slice는 문자열에서 특정한 구간의 문자열을 추출한다.

- substr
  - start 인덱스부터 시작하여 지정한 길이(length)만큼의 문자열을 반환한다.
  - 즉, 시작 위치부터 몇 개까지 자를지 선택할 수 있다.
    <br>
- slice
  - start 인덱스부터 시작하여 end 인덱스 전까지의 문자열을 반환한다.
  - 즉, 시작 위치부터 끝까지 자른다.

```jsx
let str = "안녕하세요. 행복한하루되세요!";

console.log(str.substr(3, 6)); // "세요. 행복" 출력
console.log(str.slice(9, 16)); // "한하루되세요!" 출력
```

<br>

- 짜잔!🎇

  ![](https://velog.velcdn.com/images/sieunpark/post/423a58a3-3558-4e1a-97e6-86f41a0338b9/image.png)

<br><br>

# 2. 참조

- https://opentutorials.org/course/50/97
