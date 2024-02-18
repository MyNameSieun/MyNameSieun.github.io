---
title: "[React] 비동기 통신 - axios, fetch"
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

# 1. Axios 개요

## 1.1 Axios 개념

axios 란 node.js와 브라우저를 위한 Promise 기반 http 클라이언트이다.
즉, http를 이용해서 서버와 통신하기 위해 사용하는 패키지다.

<br>

## 1.2 Axios 설치

```
yarn add axios
```

<br>

## 1.3 json-server 설정

아래와 같이 테스트용 db.json을 설정하자.

```js
{
  "todos": [
    {
      "id": "1",
      "title": "react"
    }
  ]
}
```

다음으로 json-server 설치 후, 3001번 포트로 서버를 가동시켜 사용하자.

```
yarn add json-server
```

```js
yarn json-server --watch db.json --port 3001
```

<br><br>

# 2. GET

## 2.1 Axios get

get은 서버의 데이터를 조회할 때 사용한다.

기본적인 사용 방법은 아래와 같다. [공식문서](https://axios-http.com/kr/docs/req_config)

```js
// url에는 서버의 url이 들어가고, config에는 기타 여러가지 설정을 추가할 수 있다.
// config는 axios 공식문서에서 확인할 수 있다.

axios.get(url[, config]) // GET
```

<br>

## 2.2 json-server API 명세서 확인하기⭐

Axios는 GET 요청을 할 수 있도록 도와주는 패키지일 뿐이기 때문에, Axios를 사용해서 GET 요청 코드를 작성하기에 앞서 어떤 방식으로 요청 해야할지 API 명세서를 통해 json-server의 방식을 확인해야한다.

API 명세서를 확인하면 GET 요청을 하고자할 때는 query로 보내라고 명시하고 있다.

![](/assets/images/2024/2024-02-17-12-11-12.png)

전체 정보나 상세 정보는 아래와 같이 path variable 로 url을 작성하라고 명시하고 있다.

![](/assets/images/2024/2024-02-17-14-22-44.png)

<br>

## 2.3 코드로 알아보기

json-server에 있는 todos를 axios를 이용해서 fetching하고 useState를 통해서 관리해보자.

```
yarn add axios
```

```js
// src/App.jsx

import React, { useEffect, useState } from "react";
import axios from "axios"; // axios import

const App = () => {
  const [todos, setTodos] = useState(null);

  // axios를 통해서 get 요청을 하는 함수 생성
  // 비동기처리를 해야하므로 async/await 구문을 통해서 처리
  const fetchTodos = async () => {
    const { data } = await axios.get("http://localhost:3001/todos");
    console.log("response", data);
    setTodos(data);
  };
  useEffect(() => {
    // 생성한 함수를 컴포넌트가 mount 됐을 떄 실행하기 위해 useEffect를 사용
    fetchTodos();
  }, []);

  return (
    <div>
      {todos?.map((item) => {
        return (
          <div key={item.id}>
            {item.id} : {item.title}
          </div>
        );
      })}
    </div>
  );
};
export default App;
```

생성한 Todos를 정상적으로 서버에서 가져와서 state가 set 한 것을 확인할 수 있다.

![](/assets/images/2024/2024-02-17-18-11-37.png)

![](/assets/images/2024/2024-02-17-18-06-11.png)

<br>

위 코드는 async를 사용했기 때문에 렌더링이 먼저 된 후 비동기 함수가 호출이 된다.

따라서 todos는 현재 값이 null일 수도 있는 상태이기 때문에 Optional Chaining을 넣어줘야햔다.

즉, 초기에 데이터가 없는 경우에도 애플리케이션이 오류 없이 렌더링될 수 있도록 하는 것이다.

`{todos?.map((item)`

# 3. Axios POST

## 3.1 Axios POST

post 메서드는 클라이언트에서 서버로 데이터를 보낼 때 주로 사용된다.<br>
일반적으로 데이터는 HTTP 요청의 본문(body)에 담겨 서버로 전송된다.

예를들어 사용자가 어떤 폼을 작성하고 제출할 때, 그 폼 데이터가 POST 요청으로 서버에 전송되어 데이터를 <span style="color:indianred">추가하거나 업데이트</span>할 수 있다.

```js
axios.post(url[, data[, config]])   // POST
```

<br>

input에 어떤 값을 넣고 버튼을 클릭했을 때 todo를 body에 담아 서버로 POST 요청을 보내어 값을 추가해보자.

```js
import React, { useEffect, useState } from "react";
import axios from "axios"; // axios import

const App = () => {
  const [todos, setTodos] = useState(null);
  // 새롭게 생성하는 todo를 관리하는 state
  const [inputValue, setInputValue] = useState({ title: "" });

  const fetchTodos = async () => {
    const { data } = await axios.get("http://localhost:3001/todos");
    console.log("data ", data);
    setTodos(data);
  };

  // 추가 함수
  const onSubmitHandle = async () => {
    axios.post("http://localhost:3001/todos", inputValue);

    // state를 변경하여 화면에 렌더링시켜준다.
    setTodos([...todos, inputValue]);
  };
  useEffect(() => {
    fetchTodos();
  }, []);

  return (
    <>
      <form
        onSubmit={(e) => {
          e.preventDefault();

          // 버튼 클릭 시, input에 들어있는 값(state)를 활용하여 DB에 저장(post 요청)
          onSubmitHandle();
        }}
      >
        <input
          type="text"
          value={inputValue.title}
          onChange={(e) => {
            setInputValue({ title: e.target.value });
          }}
        />
        <button>추가</button>
      </form>
      <div>
        {todos?.map((item) => {
          return (
            <div key={item.id}>
              {item.id} : {item.title}
            </div>
          );
        })}
      </div>
    </>
  );
};
export default App;
```

데이터 추가시 아래와 같은 오류가 발생하는 것을 확인할 수 있다.
![](/assets/images/2024/2024-02-17-23-58-02.png)

따라서 다시 데이터를 읽어 오는 방식으로 렌더링을 하는 방법을 선택하자

즉, axios.post 호출 후에 fetchTodos를 호출함으로써, 새로운 todo를 추가한 후에 목록을 다시 불러와서 최신 상태로 업데이트하는 것이다.

```js
// 추가 함수
const onSubmitHandle = async () => {
  axios.post("http://localhost:3001/todos", inputValue);
  // 변경 전: setTodos([...todos, inputValue]);
  fetchTodos(); // 변경 후
};
useEffect(() => {
  fetchTodos();
}, []);
```

<br><br>

# 4. Axios delete

DELETE는 저장되어 있는 데이터를 삭제하고자 요청을 보낼 때 사용한다.

```js
axios.delete(url[, config])  // DELETE
```

```js
import React, { useEffect, useState } from "react";
import axios from "axios"; // axios import

const App = () => {
  const [todos, setTodos] = useState(null);
  const [inputValue, setInputValue] = useState({ title: "" });

  // 조회 함수
  const fetchTodos = async () => {
    const { data } = await axios.get("http://localhost:3001/todos");
    console.log("data ", data);
    setTodos(data);
  };

  // 추가 함수
  const onSubmitHandle = async () => {
    axios.post("http://localhost:3001/todos", inputValue);
    fetchTodos();
  };
  useEffect(() => {
    fetchTodos();
  }, []);

  // 삭제 함수
  const onDeleteBtnHandle = async (id) => {
    axios.delete(`http://localhost:3001/todos/${id}`);
    setTodos(
      todos.filter((item) => {
        return item.id !== id;
      })
    );
  };

  return (
    <>
      <form
        onSubmit={(e) => {
          e.preventDefault();
          onSubmitHandle();
        }}
      >
        <input
          type="text"
          value={inputValue.title}
          onChange={(e) => {
            setInputValue({ title: e.target.value });
          }}
        />
        <button>추가</button>
      </form>
      <div>
        {todos?.map((item) => {
          return (
            <div key={item.id}>
              {item.id} : {item.title}
              &nbsp; <button onClick={() => onDeleteBtnHandle(item.id)}>
                삭제
              </button> {/* 삭제 버튼 추가 */}
            </div>
          );
        })}
      </div>
    </>
  );
};
export default App;
```

<br><br>

# 5. Axios patch

patch는 보통 어떤 데이터를 수정하고자 서버에 요청을 보낼 때 사용하는 메서드이다.

```js
axios.patch(url[, data[, config]])  // PATCH
```

```js
import React, { useEffect, useState } from "react";
import axios from "axios"; // axios import

const App = () => {
  const [todos, setTodos] = useState(null);
  const [inputValue, setInputValue] = useState({ title: "" });
  const [targetId, setTargetId] = useState("");
  const [contents, setContents] = useState("");

  const fetchTodos = async () => {
    const { data } = await axios.get("http://localhost:3001/todos");
    console.log("data ", data);
    setTodos(data);
  };

  // 추가 함수
  const onSubmitHandle = async () => {
    axios.post("http://localhost:3001/todos", inputValue);
    fetchTodos();
  };
  useEffect(() => {
    fetchTodos();
  }, []);

  // 삭제 함수
  const onDeleteBtnHandle = async (id) => {
    axios.delete(`http://localhost:3001/todos/${id}`);
    setTodos(
      todos.filter((item) => {
        return item.id !== id;
      })
    );
  };

  // 수정 함수
  const onUpdateBtnClickHandle = async () => {
    await axios.patch(`http://localhost:3001/todos/${targetId}`, {
      title: contents,
    });

    setTodos(
      todos.map((item) => {
        if (item.id === targetId) {
          return { ...item, title: contents };
        } else {
          return item;
        }
      })
    );
  };

  return (
    <>
      {/* 수정 폼 추가 */}
      <input
        type="text"
        placeholder="수정할 아이디"
        value={targetId}
        onChange={(e) => {
          setTargetId(e.target.value);
        }}
      />
      <input
        type="text"
        placeholder="수정할 내용"
        value={contents}
        onChange={(e) => {
          setContents(e.target.value);
        }}
      />
      <button onClick={onUpdateBtnClickHandle}>수정</button>
      <br />
      <br />
      <form
        onSubmit={(e) => {
          e.preventDefault();
          onSubmitHandle();
        }}
      >
        <input
          type="text"
          value={inputValue.title}
          onChange={(e) => {
            setInputValue({ title: e.target.value });
          }}
        />
        <button>추가</button>
      </form>
      <div>
        {todos?.map((item) => {
          return (
            <div key={item.id}>
              {item.id} : {item.title}
              &nbsp;
              <button onClick={() => onDeleteBtnHandle(item.id)}>삭제</button>
            </div>
          );
        })}
      </div>
    </>
  );
};
export default App;
```

<br>

# 6. Fetch와 axios

## 6.1 Fetch vs axios의 차이점

Fetch는 자바스크립트 내장 브라우저이기 때문에 별도의 설치 및 import를 필요로 하지 않는다.

하지만 fetch는 아래와 같은 단점이 존재하기 때문에 axios를 사용한다.

- 미지원 브라우저 존재
- 개발자에게 불친절한 response
- axios에 비해 부족한 기능

<br>

## 6.2 데이터 읽어오기

> fetch

```js
const url = "https://jsonplaceholder.typicode.com/todos";

fetch(url)
  .then((response) => response.json())
  .then(console.log);
```

- fetch().then을 한 상태여도 여전히 JSON 포맷의 응답이 아니기 때문에 response.json()을 한번 더 해주는 과정이 필요하다.
- 따라서, fetch로 데이터를 요청하는 경우 두 개의 .then()이 필요한 것이다.

<br>

> axios

```js
const url = "https://jsonplaceholder.typicode.com/todos";

axios.get(url).then((response) => console.log(response.data));
```

axios는 응답(response)을 기본적으로 JSON 포맷으로 제공하기 때문에, 단순히 response.data로만 사용하면 된다.

<br>

## 6.3 에러 처리

> fetch

```js
const url = "https://jsonplaceholder.typicode.com/todos";

axios
  .get(url)
  .then((response) => console.log(response.data))
  .catch((err) => {
    console.log(err.message);
  });
```

- fetch의 경우, catch()가 발생하는 경우는 오직 네트워크 장애 케이스이다.
- 따라서 개발자가 일일히 then() 안에 모든 케이스에 대한 HTTP 에러 처리를 해야한다.

<br>

> axios

```js
const url = "https://jsonplaceholder.typicode.com/todos";

axios
  .get(url)
  .then((response) => console.log(response.data))
  .catch((err) => {
    console.log(err.message);
  });
```

- axios.get()요청이 반환하는 Promise 객체가 갖고있는 상태코드가 2xx의 범위를 넘어가면 거부(reject)를 한다.
- 따라서, 아래와 같이 곧바로 catch() 부분을 통해 error handling이 가능하다.

```js
const url = "https://jsonplaceholder.typicode.com/todos";

// axios 요청 로직
axios
  .get(url)
  .then((response) => console.log(response.data))
  .catch((err) => {
    // 오류 객체 내의 response가 존재한다 = 서버가 오류 응답을 주었다
    if (err.response) {
      const { status, config } = err.response;

      // 없는 페이지
      if (status === 404) {
        console.log(`${config.url} not found`);
      }

      // 서버 오류
      if (status === 500) {
        console.log("Server error");
      }

      // 요청이 이루어졌으나 서버에서 응답이 없었을 경우
    } else if (err.request) {
      console.log("Error", err.message);
      // 그 외 다른 에러
    } else {
      console.log("Error", err.message);
    }
  });
```

<br>
