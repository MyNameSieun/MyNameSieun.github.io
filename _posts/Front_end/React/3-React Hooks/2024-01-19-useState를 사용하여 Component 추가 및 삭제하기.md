---
title: "[React] useState를 사용하여 Component 추가 및 삭제하기"
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

# 1. user 추가 및 삭제

## 1.1 user 추가

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
      id: users.length + 1,
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

## 1.2 user 삭제

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
      id: users.length + 1,
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

# 2. 컴포넌트 분리하기

## 2.1 user 컴포넌트 분리

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

## 2.2 button 컴포넌트 분리

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
      id: users.length + 1,
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

## 2.3 컴포넌트 파일 분리

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
