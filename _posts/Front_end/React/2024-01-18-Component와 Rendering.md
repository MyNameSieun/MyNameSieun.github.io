---
title: "[React] Component와 Rendering"
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

# 1. Component

《 [Component](https://mynamesieun.github.io/react/Component/) 》포스팅을 먼저 보고 와주세요.
{: .notice--danger}

① 이전 포스팅에서 "컴포넌트란 리액트의 핵심 빌딩 블록 중 하나로, UI 요소를 표현하는 최소한의 단위이며 화면의 특정 부분이 어떻게 생길지 정하는 <span style="color:indianred">선언체⭐</span>" 라고 하였다.

② DOM과같은 기존 명령형 프로그래밍은 브라우저에서 동적으로 변하는 UI를 표현하기 위해 직접 DOM 객체를 조작하여야했다. ((ex) `getElementById`,`appendChild`)

③ 하지만 리액트와 같은 선언형 프로그래밍은 컴포넌트를 생성하고 보여지고자 하는 UI 요소를 컴포넌트 내부에서 JSX를 통해 미리 선언해놓은 화면 요소를 리액트가 화면에 그려줄 수 있다.

<br><br>

# 2. 명령형과 선언형 프로그래밍

## 2.1 DOM (명령형 프로그래밍)

> 명령형은 어떻게(How)를 중요시 여겨서 프로그램의 제어의 흐름과 같은 방법을 제시하고 목표를 명시하지 않는 형태이다.

명령형 코드의 경우`Hello, World!`를 출력하기 위해 컴퓨터가 수행하는 절차를 일일히 코드로 작성해줘야 한다.

```jsx
const root = document.getElementById("root");
const header = document.createElement("h1");
const headerContent = document.createTextNode("Hello, World!");

header.appendChild(headerContent);
root.appendChild(header);
```

카운터 예시와 같이 격리된 예제에서는 명령형 프로그래밍 방식이 효율적인 방법일수 있지만, 복잡한 UI 시스템에서는 관리하기가 어려워진다.

<br>

## 2.2 리액트 (선언형 프로그래밍)

> 선언형은 무엇(What)을 중요시 여겨서 제어의 흐름보다는 원하는 목적을 중요시 여기는 형태이다.

React 코드의 경우 내가 UI을 선언하고 render 함수를 호출하면 React가 알아서 절차를 수행해 화면에 출력해준다.

즉, 화면에 어떻게 그려야할지는 React 내부에 잘 숨겨져 추상화되어 있는 것이다.

```jsx
const header = <h1>Hello World</h1>; // jsx
ReactDOM.render(header, document.getElementById("root"));
```

<br><br>

# 3. 렌더링 개요

## 3.1 렌더링 개념

> 컴포넌트가 <span style="color:indianred">현재 `props`와 `state`의 상태에 기초하여</span> UI를 어떻게 구성할지 컴포넌트에게 요청하는 작업을 의미한다.

- 가정
  - UI - 음식
  - 컴포넌트 - 음식을 만드는 주방장
  - 리액트 - 웨이터
    <br> <br>
- 렌더링이 일어나는 프로세스
  - 렌더링 일으키는 것은 (**triggering**) UI를 주문하고 주방으로 전달하는 것
  - 렌더링한다는 것은 (**rendering**) 주방에서 컴포넌트가 UI를 만들고 준비하는 것
  - 렌더링 결과는 실제 DOM에 커밋한다는 것은 (**commit**) - 리액트가 준비된 UI를 손님 테이블에 올려놓는 것 <br> <br>
    ![](/assets/images/2024/2024-01-18-11-29-40.png)

<br>

## 3.2 렌더링 트리거

> 렌더링이 발생하는 경우

1. 첫 리액트 앱을 실행했을 때
2. 현재 리액트 내부에 어떤 <span style="color:indianred">상태(state)에 변경이 발생</span>했을 때⭐
   1. 컴포넌트 내부 state가 변경되었을 때
   2. 컴포넌트에 새로운 props가 들어올 때
   3. 상위 부모 컴포넌트에서 위에 두 이유로 렌더링이 발생했을 때

리액트 앱이 실행되고 첫 렌더링이 일어나면 리액트는 컴포넌트의 루트에서 시작하여 아래쪽으로 쭉 훑으며 전체 컴포넌트를 렌더링하고 컴포넌트가 반환하는 JSX 결과물을 DOM 요소에 반영하여 브라우저상에 보여준다. (Virtual DOM은 나중에 다룰 것이다.)

<br>

## 3.3 리렌더링

첫 렌더링은 자동으로 일어난 것이였지만, 첫 렌더링을 끝난 이후에 추가로 렌더링을 트리거 하려면 상태를 변경해주면 된다.

setState() 이외에도 상태를 변경할 수 있는 다양한 방법이 있다. (추후 다룰 예정)

컴포넌트 상태에 변화가 생기면 리렌더링이 발생한다.

이때 여러 상태가 변경됐다면 리액트는 이를 큐 자료구조에 넣어 순서를 관리한다.

<br>

> 리렌더링의 순서

1. 리렌더링은 음식점 손님이 첫 주문 이후에 갈증이 생겨 추가로 음료를 주문하거나 처음 받은 음식이 마음에 들지 않아 새로운 메뉴를 주문하는 것과 같다.
2. 새로운 UI주문(**리렌더링**)이 일어나면 리액트가 변경된 내용을 주방에 있는 요리사인 컴포넌트에 전달하고 **컴포넌트**는 새로운 변경된 주문을 토대로 새로운 요리(**UI**)를 만든다.
3. 새롭게 만들어진 요리(**렌러딩 결과**)는 리액트에 의해 다시 손님 테이블에 올려진다.(DOM에 반영 - commit phase).<br><br>
   ![](/assets/images/2024/2024-01-18-11-30-01.png)

<br>

## 3.4 브라우저 렌더링

브라우저의 렌더링과 리액트의 렌더링은 엄연히 다른 독립적인 프로세스이다.

렌더링이 완료되고 React가 DOM을 업데이트한 후 브라우저는 화면을 그린다.

이 프로세스를 "브라우저 렌더링"이라고 하지만 혼동을 피하기 위해 "페인팅"이라고도 한다.

<br>
