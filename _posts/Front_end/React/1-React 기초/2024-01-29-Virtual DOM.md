---
title: "[React] Virtual DOM"
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

《[ DOM](https://mynamesieun.github.io/javascript/DOM/) 》에 대해 안다는 전제 하에 포스팅합니다.

<br>

# 1. Virtual DOM

리액트(react.js)나, 뷰(Vue.js)는 가상돔(Virtual DOM)을 사용해서 실제DOM을 변경하는 작업을 상당히 효율적으로 수행한다.

가상DOM은 실제 DOM과 구조가 완벽히 동일한 복사본 형태이다.

![](/assets/images/2024/2024-01-29-11-11-56.png)

실제 DOM은 아니지만, 객체(object) 형태로 메모리에 저장되기 때문에 실제 DOM을 조작하는 것 보다 훨씬 더 빠르게 조작을 수행할 수 있다.

> 즉, 실제 DOM을 조작하는 것보다, 메모리상에 올라와있는 javascript 객체를 변경하는 작업이 훨씬 더 가볍기 때문에 Virtual DOM을 사용하는 것이다.

<br>

## 1.1 DOM 조작 과정

가상 DOM이 실제 DOM을 변경하는 것은 아니라면, 어떻게 화면이 바뀌게 되는 것일까?

인스타그램의 좋아요 버튼을 눌렀다고 가정해보자. 하트의 엘리먼트 DOM 요소가 갱신되어야하기 때문에 DOM을 조작하여 화면을 바꾸어야한다.

<br>

이 상황에서 리액트는 항상 2가지 버전의 가상DOM을 가지고 있다.

1. 화면이 갱신되기 **전** 구조가 담겨있는 **가상DOM 객체**
2. 화면 갱신 **후** 보여야 할 **가상 DOM 객체**

리액트는 state가 변경돼야만 리렌더링이 되는데, 이때 2번에 해당되는 가상 DOM을 만드는 것이다.

<br>

> diffing

**state**가 변경되면 2번에서 생성된 가상돔과 1번에서 이미 갖고있었던 **가상돔을 비교**해서 어느 부분(엘리먼트)에서 변화가 일어났는지를 파악한다.

<br>

> 재조정(reconciliation)

파악이 다 끝나면, 변경이 일어난 **그 부분만** 실제 DOM에 적용시킨다.

⭐ 적용시킬 때는, 한건 한건 적용시키는 것이 아니라, 변경사항을 모두 모아 <span style="color:indianred">한 번만 적용</span>을 시킨다.(Batch Update)

<br>

## 1.2 Batch Update

리액트가 state를 batch 방법으로 update 한다.

즉, 변경된 모든 엘리먼트를 한꺼번에 반영하는 것이다.

<br>

> 클릭 한 번으로 화면에 있는 5개의 엘리먼트가 바뀌어야 한다면?

- 실제 DOM : 5번의 화면 갱신 필요
- 가상 DOM : Batch Update로 인해 단 한번만 갱신 필요

<br>
