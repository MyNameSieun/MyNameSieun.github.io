---
title: "[React] React에서 Font Awesome 사용하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 설치하기

```shell
yarn add @fortawesome/fontawesome-svg-core
yarn add @fortawesome/free-solid-svg-icons @fortawesome/free-regular-svg-icons @fortawesome/free-brands-svg-icons
yarn add @fortawesome/react-fontawesome
```

<br>

# 2. FontAwesomeIcon import

> 리액트 컴포넌트에서 Font Awesome 아이콘을 사용하기 위해 `FontAwesomeIcon` 컴포넌트를 임포트

```jsx
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
```

<br>

# 3. 아이콘 사용하기

[[폰트어썸 아이콘 검색하기↗️]](https://fontawesome.com/search)

> ① 아이콘 변수명을 카멜 표기법으로 변경

Font Awesome의 아이콘 변수명을 스네이크 표기법에서 카멜 표기법으로 변경해주자

```jsx
<FontAwesomeIcon icon="fa-regular fa-arrow-right" /> // 변경 전
<FontAwesomeIcon icon={faHouse} /> // 변경 후
```

<br>

> ② 아이콘을 import

- 변경된 변수명을 사용하기 위해, 해당 아이콘을 사용하고자 하는 컴포넌트에 import 해주면 된다.
- `solid` 부분만 사용하고자 하는 내용으로 변경하면 된다.

```jsx
import { faHouse } from "@fortawesome/free-solid-svg-icons";
```

<br>

# 4. 폰트어썸 사이즈

Font Awesome 아이콘은 아래와 같은 클래스들을 사용하여 아이콘의 크기를 조정할 수 있다.

> CSS 클래스를 사용한 사이즈 단위

- 2xs: Extra Extra Small (매우 매우 작음)
- fa-xs: Extra Small (매우 작음)
- fa-sm: Small (작음)
- fa-lg: Large (큼)
- xl: Extra Large (매우 큼)
- 2xl: 2x Extra Large (2배 매우 큼)

> size 속성을 사용한 사이즈 단위

- fa-2x: 2배 크기
- fa-3x: 3배 크기
- fa-4x: 4배 크기
- fa-5x: 5배 크기
- fa-6x: 6배 크기
- fa-7x: 7배 크기
- fa-8x: 8배 크기
- fa-9x: 9배 크기
- fa-10x: 10배 크기

```jsx
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faHouse } from "@fortawesome/free-solid-svg-icons";

const App = () => {
  return (
    <div>
      <div>
        <FontAwesomeIcon icon={faHouse} className="fa-2xs" />
        <FontAwesomeIcon icon={faHouse} className="fa-xs" />
        <FontAwesomeIcon icon={faHouse} className="fa-sm" />
        <FontAwesomeIcon icon={faHouse} className="fa-lg" />
        <FontAwesomeIcon icon={faHouse} className="fa-xl" />
        <FontAwesomeIcon icon={faHouse} className="fa-2xl" />
      </div>
      <div>
        <FontAwesomeIcon icon={faHouse} size="2x" />
        <FontAwesomeIcon icon={faHouse} size="5x" />
        <FontAwesomeIcon icon={faHouse} size="10x" />
      </div>
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-08-31-16-47-36.png)

<br>

# 5. style 적용

## 5.1 인라인 스타일 적용

> style 속성을 사용하여 CSS 스타일을 객체 형태로 직접 적용할 수 있다.

{% raw %}

```jsx
<FontAwesomeIcon icon={faAngleLeft} style={{ color: "#f34a4a" }} />
```

{% endraw %}

![](/assets/images/2024/2024-08-31-17-06-52.png)

<br>

## 5.2 styled-components 사용

> styled-components를 사용하여 아이콘의 스타일을 정의할 수 있다.

{% raw %}

```jsx
<StFontAwesomeIcon icon={faAngleLeft} />;

const StFontAwesomeIcon = styled(FontAwesomeIcon)`
  color: #f34a4a;
  font-size: 55px;
`;
```

{% endraw %}

<br>

# 5. 전체 코드

> 적용이 안 되면 유료일 가능성이 높으므로, 사용하고자 하는 아이콘이 무료인지 유료인지를 확인해보자!

```jsx
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faHouse } from "@fortawesome/free-solid-svg-icons";

const App = () => {
  return (
    <div>
      <h1>안녕!</h1>
      <FontAwesomeIcon icon={faHouse} size="2x" />
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-08-31-16-49-58.png)

<br><br>

# 참조

- [리액트에 폰트어썸 매우 쉽게 적용하기 (4단계)](https://ssddo-story.tistory.com/15)

<br>
