---
title: "[React] React Project Convention"
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

🤝 팀 프로젝트를 진행하면서 좋았던 코드 컨벤션
{: .notice--danger}

<br>

## 1. Code Convention

> React Team Project를 진행하면서 좋았던 코드 컨벤션!

① **주석은 반드시 쓸 것!!!**

<br>

② 작명 방식

| 종류            | 네이밍 규칙                                                                                                                       | 예시                                                      |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| 변수명, 함수명  | - camelCase(카멜 케이스)<br>- 함수는 `동사 + 명사` 구성                                                                           | myVariable, calculateTotal                                |
| 상수명          | SNAKE_CASE(스네이크 케이스)                                                                                                       | MAX_LENGTH, API_KEY                                       |
| 이벤트 핸들러명 | - 클래스명 : kebab-case(케밥 케이스) <br> - 컴포넌트명 : PascalCase(파스칼 케이스) <br> - 일반 파일 이름 : camelCase(카멜 케이스) | -> handleAddButton<br>-> MyComponent<br>-> userProfile.js |

- 이벤트 핸들러의 변수명은 `handle`으로 시작(onClick, onChange 등 이벤트 handleOnClick, handleOnChange)
- 반환 값이 boolean형인 함수는 `is`로 시작(모달 열려있는지? isOpen)

<br><br>

## 2. Styled-components Naming Convention

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

1. Styled 컴포넌트명 앞에 `Styled` 사용을 지양할 것
2. ~Wrapper: ~Wrapper 대신 `Box`라는 이름을 사용할 것.

<br>
