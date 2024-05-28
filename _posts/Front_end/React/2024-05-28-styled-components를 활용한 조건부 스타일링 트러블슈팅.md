---
title: "[React] styled-components를 활용한 조건부 스타일링 트러블슈팅"
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

## 트러블 슈팅💫

### 발생한 문제 🤦‍♀️

```js
<QuestionItem>
  <span>Q</span>
  <span>사이버캠퍼스? LMS? 코스모스가 뭔가요?</span>
</QuestionItem>
```

- 위 코드에서 첫 번째 span 태그만 CSS의 nth-child를 활용해서 다른 스타일링을 적용하고 싶었다.
- 하지만, styled-components를 사용하여 조건부 스타일링을 구현해야 하는 상황에서 nth-child를 사용할 수 없어 각 요소에 다른 스타일을 적용하는 것이 어려웠다.

<br>

### 문제 원인 🤷‍♀️

styled-components를 사용하여 조건부 스타일링을 구현해야 하는 상황에서는 nth-child와 같은 CSS 선택자를 직접 사용할 수 없다.

<br>

### 해결 방안 💁‍♀️

조건부 렌더링을 통해 각각의 요소에 별도의 컴포넌트로 스타일을 적용하는 방법을 활용했다.

```js
<QuestionItem>
  <QuestionBlue type="question">Q</QuestionBlue>
  <span>사이버캠퍼스? LMS? 코스모스가 뭔가요?</span>
</QuestionItem>;

const QuestionBlue = styled.div`
  color: ${(props) => (props.type === "question" ? "#3b64e6" : "#000")};
`;
```

<br>

### 주의 ⚠️

> React 컴포넌트에만 props를 전달할 수 있다.

즉, `<div>`나 `<span>`과 같은 HTML 요소에는 직접적으로 React props를 전달할 수 없는 것이다!

따라서 처음엔 아래와 같은 방법을 시도했으나,

```js
<QuestionItem>
  <span type="question">Q</span>
  <span>사이버캠퍼스? LMS? 코스모스가 뭔가요?</span>
</QuestionItem>
```

위와 같이 직접적으로 HTML 요소에 props를 전달하는 방식은 작동하지 않는다.

따라서, HTML 요소에 스타일을 적용하거나 조건부 스타일링을 구현할 때는 React 컴포넌트를 활용하여 해당 스타일을 적용해야 한다!!💡

<br>
