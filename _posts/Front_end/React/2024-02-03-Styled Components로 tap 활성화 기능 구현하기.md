---
title: "[React] Styled Components로 tap 활성화 기능 구현하기"
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

# 1. Styled Components로 tap 활성화 구현하기

> 어떤 탭이 클릭되었는지 아래처럼 style-components의 조건부 스타일링을 적용하여 구현해보자!

![](/assets/images/2024/2024-02-04-00-03-11.png)

<br>

## 1.1 적용 전 초기 코드

```js
import React, { useState } from "react";
import styled, { css } from "styled-components"; // import(retrun에 css치고 tap누르면 자동 import)

const SelectContainer = styled.div`
  width: 334px;
  background-color: #ffffff;
  display: flex;
  position: relative;
  flex-direction: column;
  margin: 40px 0px 40px 30px;
  border-radius: 13px;
  box-shadow: 0 2px 10px -7px rgba(0, 0, 0, 1);
`;
const SelectText = styled.div`
  font-size: 18px;
  margin: 30px 0 0 30px;
  font-weight: bold;
`;
const SelectList = styled.div`
  border: 2px solid #bababa;
  margin: 30px 40px -10px 40px;
  padding: 20px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  cursor: pointer;
`;

function SelectPeople() {
  const handleActivePeople = (e) => {};
  return (
    <SelectContainer>
      <SelectText>누구에게 보내실 건가요?</SelectText>
      <SelectList>강아지</SelectList>
      <SelectList>고양이</SelectList>
      <SelectList>토끼</SelectList>
      <SelectList>루돌프</SelectList>
      <SelectList>고라니</SelectList>
      <SelectList>병아리</SelectList>
    </SelectContainer>
  );
}

export default SelectPeople;
```

<br>

## 1.2 state 생성

```js
const [activePeople, setActivePeople] = useState("강아지");
```

<br>

그리고 props로 조건부 스타일을 주자<br>
스타일 컴포넌트 특성상 카멜 케이스 사용시 $를 앞에 붙여줘야한다! (ActivePeople이경우는 사용 안해도 된다.)

```js
<SelectContainer onClick={handleActivePeople}>
  <SelectText>누구에게 보내실 건가요?</SelectText>
  <SelectList $activePeople={activePeople}>강아지</SelectList>
  <SelectList $activePeople={activePeople}>고양이</SelectList>
  <SelectList $activePeople={activePeople}>토끼</SelectList>
  <SelectList $activePeople={activePeople}>루돌프</SelectList>
  <SelectList $activePeople={activePeople}>고라니</SelectList>
  <SelectList $activePeople={activePeople}>병아리</SelectList>
</SelectContainer>
```

<br>

## 1.3 props로 값 받기

이제 props로 넘긴 값을 받아보자.<br>
active할 경우 border의 색상와 텍스트 색상을 변경해줄 것이다.

```css
const SelectList = styled.div`
  border: 2px solid #bababa;
  margin: 30px 40px -10px 40px;
  padding: 20px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  cursor: pointer;
`;
```

<br>

아래와 같은 형태로 받을 수 있다.

```js
  ${props=>{}}
```

```js
const SelectList = styled.div`
  ${(props) => {
    // props.children: SelectList 컴포넌트의 자식 요소를 나타내는 속성
    // 즉, 해당 컴포넌트의 내용을 나타냄

    if (props.$activePeople === props.children) {
      return css`
        border: 2px solid #000000;
        font-weight: bold;
      `;
    }
    return css`
      border: 2px solid #bababa;
      font-weight: normal;
    `;
  }}

  margin: 30px 40px -10px 40px;
  padding: 20px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  cursor: pointer;
`;
```

<br>

## 1.4 클릭 이벤트 발생시키기

이제 클릭했을 때 클릭에 대한 이벤트를 발생시켜보자

```js
function SelectPeople() {
  const [activePeople, setActivePeople] = useState("강아지");

  const handleActivePeople = (e) => {
    // currentTarget: 이벤트 핸들러와 연결된 위치(태그), Target: 이벤트가 시작된 위치(태그)
    // 사용자가 클릭한 엘리먼트가 이벤트 리스너가 등록된 엘리먼트와 동일한지 확인
    // 동일하지 않다면(활성화되지 않은 엘리먼트일 경우) state 변경해서 활성화
    if (e.target === e.currentTarget) return;
    setActivePeople(e.target.textContent);
  };
  return (
    // onClick함수 등록
    <SelectContainer onClick={handleActivePeople}>
      <SelectText>누구에게 보내실 건가요?</SelectText>
      <SelectList $activePeople={activePeople}>강아지</SelectList>
      <SelectList $activePeople={activePeople}>고양이</SelectList>
      <SelectList $activePeople={activePeople}>토끼</SelectList>
      <SelectList $activePeople={activePeople}>루돌프</SelectList>
      <SelectList $activePeople={activePeople}>고라니</SelectList>
      <SelectList $activePeople={activePeople}>병아리</SelectList>
    </SelectContainer>
  );
}
```

<br>

부족한점

- textContent와같은 DOM API 개념 및 활용
- e.target, e.target.value, e.currentTarget의 역할과 차이점 구분

<br>
