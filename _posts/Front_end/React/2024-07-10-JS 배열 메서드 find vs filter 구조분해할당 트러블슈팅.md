---
title: "[React] JS 배열 메서드 find vs filter 구조분해할당 트러블슈팅"
categories: [React]
tag: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

## 발생한 문제 🤦‍♀️

처음엔 find 대신 filter를 사용하여 구조분해 할당을 해 주었는데, 아무것도 출력되지 않는 오류가 발생하였다.

```jsx
const { avatar, nickname, createdAt, content } = letters.filter(
  (letter) => letter.id === id
);
```

<br>

## 문제 원인 🤷‍♀️

- `filter`: 조건에 맞는 여러 항목을 찾을 때 사용하며, 반환값은 항상 배열이다.
- `find`: 조건에 맞는 첫 번째 항목을 찾을 때 사용하며, 반환값은 단일 객체이다. (조건을 만족하는 요소가 여러 개라도 첫 번째 요소만 반환한다.)

즉, filter의 반환값은 항상 배열이므로, 구조 분해 할당을 통해 원하는 요소를 바로 추출할 수 없기 때문에 오류가 발생하는 것이였다.

<br>

## 해결 방안(1) 💁‍♀️

filter로 찾은 배열의 첫 번째 요소를 추출하기 위해 [0]을 사용하는 방법

```jsx
const letter = letters.filter((letter) => letter.id === id)[0];
const { avatar, nickname, createdAt, content } = letter;
```

<br>

## 해결방안(2) 💁‍♀️

구조 분해 할당을 이용하여 배열의 첫 번째 요소를 추출하고, 다시 구조 분해 할당을 사용하여 변수에 할당하는 방법

```jsx
const [letter] = letters.filter((letter) => letter.id === id);
const { avatar, nickname, createdAt, content } = letter;
```

<br>

## 결론! 💡

조건을 만족하는 모든 요소가 필요하다면 filter를, 조건을 만족하는 하나의 요소만 필요하면 find를 사용하자!

**나는 조건을 만족하는 하나의 요소만 필요하므로 find를 사용하는 방식을 채택하였다! 🎯**

<br>
