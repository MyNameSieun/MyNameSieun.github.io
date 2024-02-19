---
title: "[React] 비동기 통신 - axios와 interceptor"
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

# 2. Axios 메소드

## 2.1 GET

get은 서버의 데이터를 조회할 때 사용한다.

기본적인 사용 방법은 아래와 같다. [공식문서](https://axios-http.com/kr/docs/req_config)

```js
// url에는 서버의 url이 들어가고, config에는 기타 여러가지 설정을 추가할 수 있다.
// config는 axios 공식문서에서 확인할 수 있다.

axios.get(url[, config]) // GET
```

<br>

> json-server API 명세서 확인하기⭐

Axios는 GET 요청을 할 수 있도록 도와주는 패키지일 뿐이기 때문에, Axios를 사용해서 GET 요청 코드를 작성하기에 앞서 어떤 방식으로 요청 해야할지 API 명세서를 통해 json-server의 방식을 확인해야한다.

API 명세서를 확인하면 GET 요청을 하고자할 때는 query로 보내라고 명시하고 있다.

![](/assets/images/2024/2024-02-17-12-11-12.png)

전체 정보나 상세 정보는 아래와 같이 path variable 로 url을 작성하라고 명시하고 있다.

![](/assets/images/2024/2024-02-17-14-22-44.png)

<br>

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

<br>

## 2.2 POST

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

<br>

## 2.3 delete

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

<br>

## 2.4 patch

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

<br><br>

# 3. Fetch와 axios

## 3.1 Fetch vs axios의 차이점

Fetch는 자바스크립트 내장 브라우저이기 때문에 별도의 설치 및 import를 필요로 하지 않는다.

하지만 fetch는 아래와 같은 단점이 존재하기 때문에 axios를 사용한다.

- 미지원 브라우저 존재
- 개발자에게 불친절한 response
- axios에 비해 부족한 기능
- 직접 JSON으로 변환 필요

<br>

## 3.2 데이터 읽어오기

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

## 3.3 에러 처리

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

## 3.4 .env를 활용한 리팩토링

.env파일을 생성하여 서버 주소를 환경 변수로 관리하자.

```
// .env

REACT_APP_SERVER_URL=http://localhost:3001
```

<br>

이제 프로젝트 어디에서든 process.env 객체를 통해 환경 변수에 접근할 수 있다.

> 변경 전

```js
("http://localhost:3001/todos");
```

<br>

> 변경 후

```js
`${process.env.REACT_APP_SERVER_URL}/todos`;
```

<br><br>

# 4. axios interceptor 개요

## 4.1 axios interceptor 개념

Axios 라이브러리에서 제공하는 기능 중 하나로, Axios 요청 및 응답을 가로채어 변형하는 역할을 한다.

요청 헤더 수정, 인증 토큰 추가, 로그 관련 로직 삽입, 에러 핸들링 등 요청 및 응답시에 필요한 작업들을 한꺼번에 처리를 할 수 있다.

(1) 요청(request)이 처리되기 전( = http request가 서버에 전달되기 전)<br>
(2) 응답(response)의 then(=성공) 또는 catch(=실패)가 처리되기 전

![](/assets/images/2024/2024-02-18-23-06-01.png)
[출처 : https://javascript.plainenglish.io/how-to-implement-a-request-interceptor-like-axios-896a1431304a]

<br><br>

# 5. axios interceptor 사용 예제

## 5.1 instance 만들기, baseURL 설정하기

지금까지 custom 설정이 전혀 되어있지 않은 axios를 통해 데이터 통신을 했다.<br>
이 axios를 인스턴스(instance)라고 한다.

```js
const data = axios.get("http://localhost:4000/");
```

<br>

json-server 설정을 먼저 한 후, instance를 만들어 가공해보자.

```js
// src > axios > api.js

import axios from "axios";

// axios.create의 입력값으로 들어가는 객체는 configuration 객체이다.
// https://axios-http.com/docs/req_config 참조

const instance = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL,
});

export default instance;
```

<br>

api.js에 가공한 axios를 만들었으니, 가공한 axios를 import해주면 된다.

```js
// App.jsx
import api from "./axios/api"; // import

// 변경 전
const fetchTodos = async () => {
  const { data } = await axios.get(`${process.env.REACT_APP_SERVER_URL}/todos`);
  console.log("data ", data);
  setTodos(data);
};

// 변경 후
// (1) axios를 api로 변경
// (2) baseURL 있으므로 기존 URL 코드 삭제
const fetchTodos = async () => {
  const { data } = await api.get("todos");
  console.log("data ", data);
  setTodos(data);
};
```

<br>

나머지 부분도 변경해주도록하자.

```js
import React, { useEffect, useState } from "react";
import api from "./axios/api"; // import

const App = () => {
  const [todos, setTodos] = useState(null);
  const [inputValue, setInputValue] = useState({ title: "" });
  const [targetId, setTargetId] = useState("");
  const [contents, setContents] = useState("");

  const fetchTodos = async () => {
    const { data } = await api.get(`todos`);
    // console.log("data ", data);
    setTodos(data);
  };

  // 추가 함수
  const onSubmitHandle = async () => {
    api.post("/todos", inputValue);
    fetchTodos();
  };
  useEffect(() => {
    fetchTodos();
  }, []);

  // 삭제 함수
  const onDeleteBtnHandle = async (id) => {
    api.delete(`/todos/${id}`);
    setTodos(
      todos.filter((item) => {
        return item.id !== id;
      })
    );
  };

  // 수정 함수
  const onUpdateBtnClickHandle = async () => {
    await api.patch(`/todos/${targetId}`, {
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
    ...
  )
  };
export default App;

```

get 요청하는 부분이 간결해진 것을 확인할 수 있다.

이제부턴, 서버가 변경되어도 api.js 파일만 수정해주면 된다!

<br>

## 5.2 request, response에 적용하기

요청을 보낼 때, 그리고 서버로부터 응답을 받을 때(실패할 때) 특정한 일을 수행해야 한다면 아래와 같이 처리하면 된다.

```js
import axios from "axios";

const instance = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL,
});

instance.interceptors.request.use(
  // 요청을 보내기 전 수행되는 함수
  function () {},

  // 오류 요청을 보내기 전 수행되는 함수
  function () {}
);

instance.interceptors.response.use(
  // 응답을 내보내기 전 수행되는 함수
  function () {},

  // 오류 응답을 내보내기 전 수행되는 함수
  function () {}
);
export default instance;
```

<br>

위 코드를 채워보자.

```js
// src > axios > api.js

import axios from "axios";

const instance = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL,
});

instance.interceptors.request.use(
  // 요청을 보내기 전 수행되는 함수
  function (config) {
    console.log(`인터셉터 요청 성공`);
    return config;
  },

  // 오류 요청을 보내기 전 수행되는 함수
  function (error) {
    console.log(`인터셉터 요청 오류`);
    return Promise.reject(error);
  }
);

instance.interceptors.response.use(
  // 응답을 내보내기 전 수행되는 함수
  function (response) {
    console.log(`인터셉터 응답 받음`);
    return response;
  },

  // 오류 응답을 내보내기 전 수행되는 함수
  function (error) {
    console.log(`인터셉터 응답 오류 발생`);
    return Promise.reject(error);
  }
);
export default instance;
```

![](/assets/images/2024/2024-02-19-02-24-21.png)

브라우저에서 로그를 확인해보면 요청과 응답 중간에 가로채서 어떠한 작업을 수행해 주는 것을 볼 수 있다.

<br>

## 5.3 실패 시켜보기

instance의 설정을 변경시켜서 요청을 실패시켜보자.

```js
import axios from "axios";

const instance = axios.create({
  baseURL: process.env.REACT_APP_SERVER_URL,
  timeout: 1,
});

export default instance;
```

이렇게 변경해주게 되면, 요청 타임아웃이 1ms(1 밀리세컨)으로 매우 짧은 시간이기 때문에 서버에서 응답을 받기 전에 오류가 발생하게 된다.

![](/assets/images/2024/2024-02-19-02-25-54.png)
