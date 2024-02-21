---
title: "[React] Styled-components Naming Convention"
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

프로젝트를 진행하면서 Styled-components를 효율적이고 깔끔하게 작성하고 싶었다.<br>
따라서 여러 방법을 적용하면서 나에게 적합한 컨벤션을 찾았다.

<br>

## Styled-components Naming Convention

- 최상위 부모
  - `컴포넌트명 + Layout`<br><br>
- 최상위 부모의 자식 (optional)
  - `컴포넌트명 + Container`
  - `컴포넌트명 + Row/Col` (Row는 가로, Col은 세로)<br><br>
- 나머지 요소들
  - div 태그: `컴포넌트명 + Box`
  - section 태그: ` 컴포넌트명 + Section`
  - ul 태그: `컴포넌트명 + List`
  - li 태그: `컴포넌트명 + Item`
  - p 태그: `컴포넌트명 + Paragraph`
  - span 태그: `컴포넌트명 + Span` or `컴포넌트명 + Text`

<br>

➡️ Layout > Container > Box 순으로 작성하면 된다.

- Layout : 모든 요소를 감쌀 때
- Container : 여러 개의 요소를 감쌀 때
- Box : 한 개의 요소를 감쌀 때

```js
// 예시
import styled from "styled-components";

import Navbar from "components/common/Navbar";
import Tabs from "components/Tabs";
import AddForm from "components/AddForm";
import LetterList from "components/LetterList";

function Home() {
  return (
    <HomeLayout>
      <Navbar />
      <HomeRow>
        <Tabs />
        <HomeCol>
          <AddForm />
          <LetterList />
        </HomeCol>
      </HomeRow>
    </HomeLayout>
  );
}

const HomeLayout = styled.div`
  max-width: 1200px;
  margin: auto;
`;
const HomeRow = styled.div`
  display: flex;
  flex-direction: row;
  max-height: 750px;
  width: 100%;
`;
const HomeCol = styled.div`
  display: flex;
  flex-direction: column;
  max-height: 750px;
  width: 100%;
`;

export default Home;
```

<br>

> 지양해야 할 사항

1. Styled'컴포넌트명': 앞에 Styled 사용을 지양할 것
2. ~Wrapper: ~Wrapper 대신 Box라는 이름을 사용할 것.

<br><br>

## 참조

- [복잡한 styled-components 구조 개선해보기](https://velog.io/@hayoung474/Front-End-%EB%B3%B5%EC%9E%A1%ED%95%9C-styled-components-%EA%B5%AC%EC%A1%B0-%EA%B0%9C%EC%84%A0%ED%95%B4%EB%B3%B4%EA%B8%B0)
- [Styled Components : Naming Convention](https://github.com/Hi-Fi-Club/FE/wiki/Styled-Components-%3A-Naming-Convention)
