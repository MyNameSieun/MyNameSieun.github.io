---
title: "[React] Styling과 map"
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

이번 포스팅에서는 컴포넌트에 CSS를 적용해서 꾸며볼 것이다!

아래와 같이 입력해 새로운 프로젝트를 생성해보자.

```js
yarn create react-app 프로젝트이름
```

<br>

# 1. 컴포넌트 스타일링

## 1.1 컴포넌트 구현하기

> CSS 치트 시트를 활용해서 아래 이미지와 똑같이 만들어보자

![](/assets/images/2024/2024-01-19-00-06-41.png)

- CSS 치트시트
  - padding
  - display : flex
    - alignItems
    - justifyContent
- gap
- width, height
- border
- borderRadius

<br>

> 구현 결과

{% raw %}

```js
// App.js
import React from "react";

const App = () => {
  const style = {
    padding: "100px",
    display: "flex",
    gap: "12px",
  };

  const squareStyle = {
    width: "100px",
    height: "100px",
    border: "1px solid green",
    borderRadius: "10px",
    display: "flex",
    alignItems: "center",
    justifyContent: "center",
  };

  return (
    <div style={style}>
      <div style={squareStyle}>감자</div>
      <div style={squareStyle}>고구마</div>
      <div style={squareStyle}>오이</div>
      <div style={squareStyle}>가지</div>
      <div style={squareStyle}>옥수수</div>
    </div>
  );
};

export default App;
```

{% endraw %}

<br>

## 1.2 CSS 코드 분리하기

> 지금까지 작성했던 js파일에서 CSS를 별도의 모듈(파일)로 분리해서 사용해보자

JSX는 HTML과 비슷하지만 class가 아닌 className 을 사용한다.

{% raw %}

```js
// App.js
import React from "react";
import "./App.css"; //  App.css 파일을 import 해줘야한다.

const App = () => {
  return (
    <div className="app-style">
      <div className="component-style">감자</div>
      <div className="component-style">고구마</div>
      <div className="component-style">오이</div>
      <div className="component-style">가지</div>
      <div className="component-style">옥수수</div>
    </div>
  );
};

export default App;
```

{% endraw %}

<br>

```css
/* App.css */
.app-style {
  padding: 100px;
  display: flex;
  gap: 12px;
}
.component-style {
  width: 100px;
  height: 100px;
  border: 1px solid green;
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

<br><br>

# 2. 반복되는 컴포넌트 처리하기

## 2.1 map()

> 목차1의 App.js에서 작성한 `<div className="component-style">`처럼 컴포넌트가 계속 중복되는 것을 확인할 수 있는데, map()메서드로 반복되는 컴포넌트를 처리하는 방법에 대해 알아보자!

{% raw %}

```js
// App.js
import React from "react";
import "./App.css";

const App = () => {
  // map() 을 사용하기 위해 채소들의 이름을 배열로 만들었다.
  const vegetables = ["감자", "고구마", "오이", "가지", "옥수수"];

  return (
    <div className="app-style">
      {vegetables.map(function (item) {
        return <div className="component-style">{item}</div>;
      })}
    </div>
  );
};

export default App;
```

{% endraw %}

① JSX 부분에서 js 코드인 `map()`을 작성할 것 이기때문에 `{ }` 로 먼저 감싸고 시작한다.

② JSX에서 `map()` 은 배열의 모든 요소를 순회하기때문에 클라이언트에서는 배열 형태의 데이터를 활용해서 화면을 그려주는 경우가 많고, 이때 배열의 값들로 동적으로 컴포넌트를 만들 수 있다.

③ 위 코드처럼 map을 사용하니 중복된 코드가 사라지고 1개의 컴포넌트를 이용하면서 그 안에서 `<div>{vegetableName}</div>` 가 순차적으로 보여지고 있다.

→ 이처럼 map() 의 기능을 이용해서 반복되는 컴포넌트를 간단하게 화면에 표시할 수 있는 것이다!

<br>

> 위 코드를 화살표 함수로 표현해보자

{% raw %}

```js
const App = () => {
  const vegetables = ["감자", "고구마", "오이", "가지", "옥수수"];

  return (
    <div className="app-style">
      {vegetables.map((item) => {
        return <div className="component-style">{item}</div>;
      })}
    </div>
  );
};
```

{% endraw %}

<br>

## 2.1 filter()

> filter() 메서드를 사용해 오이가 아닌 것만 출력해보자.

{% raw %}

```js
// App.js
import React from "react";
import "./App.css";

const App = () => {
  const vegetables = ["감자", "고구마", "오이", "가지", "옥수수"];

  return (
    <div className="app-style">
      {vegetables
        .filter((item) => {
          return item !== "오이";
        })
        .map(function (item) {
          return <div className="component-style">{item}</div>;
        })}
    </div>
  );
};

export default App;
```

{% endraw %}

![](/assets/images/2024/2024-01-19-02-43-01.png)

① `filter()` 메서드는 새로운 배열을 반환한다.

② 따라서, `배열.map()` 메서드를 용하여 필터링된 배열의 요소들을 변환하는 것과 동일한 결과를 얻을 수 있다.

③ 이를 통해 "오이"를 제외한 채소들을 `<div>` 요소로 변환한 새로운 배열을 생성할 수 있는 것이다.

<br><br>

# 3. 객체가 담긴 배열 다루기

user 라는 정보가 주어지고, 배열안에 object literal 형태의 데이터가 있을 때

{% raw %}

```js
const users = [
  { id: 1, age: 30, name: "송중기" },
  { id: 2, age: 24, name: "송강" },
  { id: 3, age: 21, name: "김유정" },
  { id: 4, age: 29, name: "구교환" },
];
```

{% endraw %}

아래 이미지가 출력되도록 코드를 작성해보자

![](/assets/images/2024/2024-01-19-02-56-43.png)

{% raw %}

```jsx
// app.js
import React from "react";
import "./App.css";

const App = () => {
  const users = [
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ];
  return (
    <div className="app-style">
      <div className="component-style">
        {users[0].age}-{users[0].name}
      </div>
      <div className="component-style">
        {users[1].age}-{users[1].name}
      </div>
      <div className="component-style">
        {users[2].age}-{users[2].name}
      </div>
      <div className="component-style">
        {users[3].age}-{users[3].name}
      </div>
    </div>
  );
};

export default App;
```

{% endraw %}

위 코드를 함수를 이용해서 리팩토링해보자. 목차2에서 했던 것처럼 map()을 사용하면 된다.

{% raw %}

```js
// app.js
import React from "react";
import "./App.css";

const App = () => {
  const users = [
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ];
  return (
    <div className="app-style">
      {/* js이므로 중괄호를 써줘야한다. */}
      {users.map(function (item) {
        return (
          // 멀티라인일 경우 소괄호 써줘야한다.
          <div className="component-style">
            {item.age}-{item.name}
          </div>
        );
      })}
    </div>
  );
};

export default App;
```

{% endraw %}

App 컴포넌트에서 `user.map()` 을 통해 user의 정보를 순회하고 각각의 user 정보를 User 컴포넌트로 주입해준다.

<br>

> 오류 해결하기

map() 사용시, 콘솔 창에 다음과 같은 경고가 뜨는 것을 확인할 수 있다.

![](/assets/images/2024/2024-01-19-03-28-05.png)

리스트가 모든 i들은 key라는 prop이 필요하다는 경고이다.

⚠️ 즉, map()을 사용할때는 `key={item.id}`와 같이 반복 요소에 대해 id를 붙여줘야한다.

{% raw %}

```jsx
const App = () => {
  const users = [
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ];
  return (
    <div className="app-style">
      {/* js이므로 중괄호를 써줘야한다. */}
      {users.map(function (item) {
        return (
          // 멀티라인일 경우 소괄호 써줘야한다.
          <div key={item.id} className="component-style">
            {item.age}-{item.name}
          </div>
        );
      })}
    </div>
  );
};
```

{% endraw %}

<br>
