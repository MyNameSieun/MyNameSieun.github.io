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

<br>

## 1.2 CSS 코드 분리하기

> 지금까지 작성했던 js파일에서 CSS를 별도의 모듈(파일)로 분리해서 사용해보자

JSX는 HTML과 비슷하지만 class가 아닌 className 을 사용한다.

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

① JSX 부분에서 js 코드인 `map()`을 작성할 것 이기때문에 `{ }` 로 먼저 감싸고 시작한다.

② JSX에서 `map()` 은 배열의 모든 요소를 순회하기때문에 클라이언트에서는 배열 형태의 데이터를 활용해서 화면을 그려주는 경우가 많고, 이때 배열의 값들로 동적으로 컴포넌트를 만들 수 있다.

③ 위 코드처럼 map을 사용하니 중복된 코드가 사라지고 1개의 컴포넌트를 이용하면서 그 안에서 `<div>{vegetableName}</div>` 가 순차적으로 보여지고 있다.

→ 이처럼 map() 의 기능을 이용해서 반복되는 컴포넌트를 간단하게 화면에 표시할 수 있는 것이다!

<br>

> 위 코드를 화살표 함수로 표현해보자

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

<br>

## 2.1 filter()

> filter() 메서드를 사용해 오이가 아닌 것만 출력해보자.

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

![](/assets/images/2024/2024-01-19-02-43-01.png)

① `filter()` 메서드는 새로운 배열을 반환한다.

② 따라서, `배열.map()` 메서드를 용하여 필터링된 배열의 요소들을 변환하는 것과 동일한 결과를 얻을 수 있다.

③ 이를 통해 "오이"를 제외한 채소들을 `<div>` 요소로 변환한 새로운 배열을 생성할 수 있는 것이다.

<br><br>

# 3. 객체가 담긴 배열 다루기

user 라는 정보가 주어지고, 배열안에 object literal 형태의 데이터가 있을 때

```js
const users = [
  { id: 1, age: 30, name: "송중기" },
  { id: 2, age: 24, name: "송강" },
  { id: 3, age: 21, name: "김유정" },
  { id: 4, age: 29, name: "구교환" },
];
```

아래 이미지가 출력되도록 코드를 작성해보자

![](/assets/images/2024/2024-01-19-02-56-43.png)

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

위 코드를 함수를 이용해서 리팩토링해보자. 목차2에서 했던 것처럼 map()을 사용하면 된다.

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

App 컴포넌트에서 `user.map()` 을 통해 user의 정보를 순회하고 각각의 user 정보를 User 컴포넌트로 주입해준다.

<br>

> 오류 해결하기

map() 사용시, 콘솔 창에 다음과 같은 경고가 뜨는 것을 확인할 수 있다.

![](/assets/images/2024/2024-01-19-03-28-05.png)

리스트가 모든 i들은 key라는 prop이 필요하다는 경고이다.

⚠️ 즉, map()을 사용할때는 `key={item.id  }`와 같이 반복 요소에 대해 id를 붙여줘야한다.

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

<br><br>

# 4. list 변경

## 4.1 user 추가

![](/assets/images/2024/2024-01-19-02-56-43.png)

위 사진과 같을때, user를 추가하는 연습을 해보자.

> 추가 버튼을 누르고 없어지게 하려면 렌더링이 되야하기 때문에 state를 사용해주도록하자.

```jsx
// app.js
import React, { useState } from "react";
import "./App.css"; // App.css 파일을 import 해줘야 한다.

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

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

export default App;
```

<br>

> 이제 UI를 작성해주도록 하자.

나이와 이름 등록을 위해 `input`태그를 작성한 후, useState를 통해 유저의 입력값을 담을 상태를 명시해주도록 하자.

💬 value와 onChange는 항상 페어라는 것을 기억하자!

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  return (
    <div>
      <div>
        이름 : &nbsp;{" "}
        <input value={name} onChange={(event) => setName(event.target.value)} />
        <br />
        나이 :&nbsp;{" "}
        <input
          value={age}
          onChange={(event) => {
            setAge(event.target.value);
          }}
        />
      </div>
      <div className="app-style">
        {users.map(function (item) {
          return (
            <div key={item.id} className="component-style">
              {item.age}-{item.name}
            </div>
          );
        })}
      </div>
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-19-10-28-14.png)

<br>

위 코드에서는 onChange를 인라인으로 작성해주었는데 웬만하면 변수로 할당해주는게 더 좋다고 한다.

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  const nameChangeHandler = (event) => {
    setName(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setAge(event.target.value);
  };

  return (
    <div>
      <div>
        이름 : &nbsp; <input value={name} onChange={nameChangeHandler} />
        <br />
        나이 :&nbsp; <input value={age} onChange={ageChangeHandler} />
      </div>
      <div className="app-style">
        {users.map(function (item) {
          return (
            <div key={item.id} className="component-style">
              {item.age}-{item.name}
            </div>
          );
        })}
      </div>
    </div>
  );
};

export default App;
```

<br>

> 이제 버튼 클릭시 user가 추가되도록 해보자

먼저 버튼을 만든 후 버튼을 누르면 액션이 일어나도록 하기 위해 `handler()` 함수를 만들어줬다.

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  const nameChangeHandler = (event) => {
    setName(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setAge(event.target.value);
  };

  // 추가 버튼 클릭
  const clickAddBtnHandler = () => {
    // 1. 새로운 형태의 user 정보를 만든다.
    // user :  { id: 1, age: 30, name: "송중기" }
    // 2. 새로 생성된 user을 배열에 더한다.
    // 3. 단, 더할 때는 불변성을 유지하기 위해 스프레드 오퍼레이션을 사용하여 원래 있던 배열을 풀어야한다.

    const newUsers = {
      id: users.lenght + 1,
      age,
      name,
    };

    setUsers([...users, newUsers]); // 불변성 유지
  };

  return (
    <div>
      <div>
        이름 : &nbsp; <input value={name} onChange={nameChangeHandler} />
        <br />
        나이 :&nbsp; <input value={age} onChange={ageChangeHandler} />
        <br />
        <button onClick={clickAddBtnHandler}>추가</button>
      </div>
      <div className="app-style">
        {users.map(function (item) {
          return (
            <div key={item.id} className="component-style">
              {item.age}-{item.name}
            </div>
          );
        })}
      </div>
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-19-10-50-06.png)

<br>

## 4.2 user 삭제

> 삭제를 구현하기 위해 먼저 삭제 버튼을 만들어 주자.

```js
<div className="app-style">
  {users.map(function (item) {
    return (
      <div key={item.id} className="component-style">
        {item.age}-{item.name}
        <button>x</button>
      </div>
    );
  })}
</div>
```

![](/assets/images/2024/2024-01-19-11-44-59.png)
<br>

.

> 그 후, 삭제를 구현하는 함수인 `clickRemoveBtnHandler()`를 만들어주자.

삭제를 하기 위해 각 item에 대한 제어가 있어어야 하기 때문에 `filter()` 메서드를 사용하여 user의 id가 어떤 값이랑 같지 않은 값만 필터링을 하면 된다.

```js
users.filter(user=>user.id ! == 어떤 값)
```

<br>

이 어떤 값을 세팅하기 위해 버튼을 누를 때 어떤 값을 넘겨주면 된다! (어떤 값 = 넘겨 준 id 값)

따라서 삭제 버튼에 user을 식별할 수 있는 값인 `item.id`를 매개 변수로 넣어주고 `clickRemoveBtnHandler()`에서 `item.id`를 받아 필터링을 해주면 되는 것이다.

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  const nameChangeHandler = (event) => {
    setName(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setAge(event.target.value);
  };

  // 추가 버튼 클릭
  const clickAddBtnHandler = () => {
    const newUsers = {
      id: users.lenght + 1,
      age,
      name,
    };

    setUsers([...users, newUsers]); // 불변성 유지
  };

  // 삭제버튼 클릭
  const clickRemoveBtnHandler = (아이디) => {
    const newUsers = users.filter((user) => user.id !== 아이디);
    setUsers(newUsers);
  };

  return (
    <div>
      <div>
        이름 : &nbsp; <input value={name} onChange={nameChangeHandler} />
        <br />
        나이 :&nbsp; <input value={age} onChange={ageChangeHandler} />
        <br />
        <button onClick={clickAddBtnHandler}>추가</button>
      </div>
      <div className="app-style">
        {users.map(function (item) {
          return (
            <div key={item.id} className="component-style">
              {item.age}-{item.name}
              {/* 
              함수 `()=>`로 감싸주지 않으면 실행되버리기 때문에 꼭 감싸줘야한다. */}
              <button onClick={() => clickRemoveBtnHandler(item.id)}>x</button>
            </div>
          );
        })}
      </div>
    </div>
  );
};

export default App;
```

![](/assets/images/2024/2024-01-19-12-03-57.png)

<br><br>

# 5. 컴포넌트 분리하기

## 5.1 user 컴포넌트 분리

아래 코드를 하나의 컴포넌트로 관리해주도록 하자.

초록색 박스 하나에 해당하는 부분이다.

```js
return (
  <div key={item.id} className="component-style">
    {item.age}-{item.name}
    <button onClick={() => clickRemoveBtnHandler(item.id)}>x</button>
  </div>
);
```

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  const nameChangeHandler = (event) => {
    setName(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setAge(event.target.value);
  };

  // 추가 버튼 클릭
  const clickAddBtnHandler = () => {
    const newUsers = {
      id: users.lenght + 1,
      age,
      name,
    };
    setUsers([...users, newUsers]);
  };

  // 삭제버튼 클릭
  const clickRemoveBtnHandler = (아이디) => {
    const newUsers = users.filter((user) => user.id !== 아이디);
    setUsers(newUsers);
  };

  return (
    <div>
      <div>
        이름 : &nbsp; <input value={name} onChange={nameChangeHandler} />
        <br />
        나이 :&nbsp; <input value={age} onChange={ageChangeHandler} />
        <br />
        <button onClick={clickAddBtnHandler}>추가</button>
      </div>
      <div className="app-style">
        {users.map(function (item) {
          // 아래와 같이 props에 내려주면 된다.
          return (
            <User
              key={item.id}
              item={item}
              removeFunction={clickRemoveBtnHandler}
            />
          );
        })}
      </div>
    </div>
  );
};

const User = ({ item, removeFunction }) => {
  //item을 구조분해 할당을 통해 받아줬다.

  return (
    <div key={item.id} className="component-style">
      {item.age}-{item.name}
      <button onClick={() => removeFunction(item.id)}>x</button>
    </div>
  );
};

export default App;
```

<br>

# 5.2 button 컴포넌트 분리

```js
// app.js
import React, { useState } from "react";
import "./App.css";

const App = () => {
  const [users, setUsers] = useState([
    { id: 1, age: 30, name: "송중기" },
    { id: 2, age: 24, name: "송강" },
    { id: 3, age: 21, name: "김유정" },
    { id: 4, age: 29, name: "구교환" },
  ]);

  const [name, setName] = useState("");
  const [age, setAge] = useState("");

  const nameChangeHandler = (event) => {
    setName(event.target.value);
  };

  const ageChangeHandler = (event) => {
    setAge(event.target.value);
  };

  // 추가 버튼 클릭
  const clickAddBtnHandler = () => {
    const newUsers = {
      id: users.lenght + 1,
      age,
      name,
    };
    setUsers([...users, newUsers]); // 불변성 유지
  };

  // 삭제버튼 클릭
  const clickRemoveBtnHandler = (아이디) => {
    const newUsers = users.filter((user) => user.id !== 아이디);
    setUsers(newUsers);
  };

  return (
    <div>
      <div>
        이름 : &nbsp; <input value={name} onChange={nameChangeHandler} />
        <br />
        나이 :&nbsp; <input value={age} onChange={ageChangeHandler} />
        <br />
        {/* props 넘겨준다 */}
        <Button clickAddBtnHandler={clickAddBtnHandler} />
      </div>
      <div className="app-style">
        {users.map(function (item) {
          return (
            <User
              key={item.id}
              item={item}
              removeFunction={clickRemoveBtnHandler}
            />
          );
        })}
      </div>
    </div>
  );
};

const User = ({ item, removeFunction }) => {
  return (
    <div key={item.id} className="component-style">
      {item.age}-{item.name}
      <button onClick={() => removeFunction(item.id)}>x</button>
    </div>
  );
};
//item을 구조분해 할당을 통해 받아줬다.
const Button = ({ clickAddBtnHandler }) => {
  return <button onClick={clickAddBtnHandler}>추가</button>;
};

export default App;
```

<br>

## 5.3 컴포넌트 파일 분리

> 위에서 생성한 컴포넌트를 새로운 파일로 분리해보자

먼저 아래와 같이 파일을 생성하자<br>
⚠️ 협업할 때 어떤 파일이 컴포넌트 파일인지 명확히 하기 위해 .js보단 .jsx로 파일을 지정해주는 것이 좋다.

![](/assets/images/2024/2024-01-19-14-04-12.png)

<br>

그 후, `import`, `export` 해주면 된다.

```jsx
// app.jsx
import React, { useState } from "react";
import "./App.css";
import Button from "./components/Button";
import User from "./components/User";
```

```jsx
// src/User.jsx
const User = ({ item, removeFunction }) => {
  return (
    <div key={item.id} className="component-style">
      {item.age}-{item.name}
      <button onClick={() => removeFunction(item.id)}>x</button>
    </div>
  );
};

export default User;
```

```jsx
// src/Button.jsx
const Button = ({ clickAddBtnHandler }) => {
  return <button onClick={clickAddBtnHandler}>추가</button>;
};

export default Button;
```

<br>
