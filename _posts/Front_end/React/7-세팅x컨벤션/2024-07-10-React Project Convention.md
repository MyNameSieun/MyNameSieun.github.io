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

# 1. Code Convention

> React Team Project를 진행하면서 좋았던 코드 컨벤션!

① **주석은 반드시 쓸 것!!!**

② 작명 방식

| 케이스       | 사용 예시                                        | 예시                                |
| ------------ | ------------------------------------------------ | ----------------------------------- |
| `camelCase`  | - 변수명, 함수명 <br>- 함수는 `동사 + 명사` 구성 | `calculateTotal`, `handleAddButton` |
| `kebab-case` | CSS 클래스명, 폴더명, 컴포넌트가 아닌 JS 파일명  | `main-container`, `utils-date.js`   |
| `PascalCase` | React 컴포넌트명(.jsx)                           | `App.jsx`, `TodoList.jsx`,          |
| `SNAKE_CASE` | 상수명                                           | `MAX_LENGTH`, `API_KEY`             |

- 이벤트 핸들러의 변수명은 `handle`으로 시작(onClick, onChange 등 이벤트 handleOnClick, handleOnChange)
- 반환 값이 boolean형인 함수는 `is`로 시작(모달 열려있는지? isOpen)

<br><br>

# 2. 변수와 함수 네이밍

| 유형 | 규칙                                                                                                                                                                                                                                        |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 공통 | - 줄임말 사용하지 않음(`TDItem` → `todoItem` )<br> - 길어도 좋으며 명확하게 작성(`addEventListener`)<br> - 2개 이상의 단어 사용(`list` → `todoList`)<br> - 단수와 복수 구분 (`card`/ `cards`, `getElementById` /`getElementsByClassName ` ) |
| 변수 | - 담는 값의 타입이 무엇인지 파악 후 명확하게 작성<><br> -- HTML 요소를 가져올 경우 `Element` 추가(`cardElement` / `cardElements`)<br> -- 배열일 경우에는 `list` 나 `복수(s)`를, 객체일 경우에는 `object` 를 붙이기                          |
| 함수 | - 동사 + 명사 사용<br> - 가끔 `on` 또는 `handle` 추가                                                                                                                                                                                       |

<br><br>

## 3. Styled-components Naming Convention

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
