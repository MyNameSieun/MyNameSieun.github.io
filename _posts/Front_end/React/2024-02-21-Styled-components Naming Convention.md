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

> 태그명은 S + PascaleCase으로 작성한다.

```js
export const SSection = styled.div`
  // code
`;

export const SHeader = styled.header`
  // code
`;

export const SSection = styled.section`
  // code
`;

export const SInput = styled.input`
  // code
`;
```

<br>

> Styled-components Naming Convention

- 최상위 부모 : `S + Layout`<br><br>
- 최상위 부모의 자식 : `S + Container`<br><br>
- 나머지 요소들
  - div 태그: `S + Box`
  - section 태그: `S + Section`
  - ul 태그: `S + List`
  - li 태그: `S + Item`
  - p 태그: `S + Paragraph`
  - span 태그: `S + Span` or `S + Text`

<br>

➡️ SLayout > SContainer > SBox 순으로 작성하면 된다.

- Layout : 모든 요소를 감쌀 때
- Container : 여러 개의 요소를 감쌀 때
- Box : 한 개의 요소를 감쌀 때

<br>

> 지양해야 할 사항

1. Styled'컴포넌트명': 앞에 Styled 사용을 지양할 것
2. ~Wrapper: ~Wrapper 대신 Box라는 이름을 사용할 것.

<br>

# 참조

- [복잡한 styled-components 구조 개선해보기](https://velog.io/@hayoung474/Front-End-%EB%B3%B5%EC%9E%A1%ED%95%9C-styled-components-%EA%B5%AC%EC%A1%B0-%EA%B0%9C%EC%84%A0%ED%95%B4%EB%B3%B4%EA%B8%B0)
- [Styled Components : Naming Convention](https://github.com/Hi-Fi-Club/FE/wiki/Styled-Components-%3A-Naming-Convention)
- [나만의 코딩 컨벤션 (react)](https://2mojurmoyang.tistory.com/178)
