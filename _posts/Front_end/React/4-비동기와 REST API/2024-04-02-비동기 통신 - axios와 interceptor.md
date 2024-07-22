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
yarn global add json-server
```

```js
yarn json-server --watch db.json --port 3001
```

<br><br>

# 2. Axios 메소드

## 2.1 GET

> get은 서버의 데이터를 조회할 때 사용한다.

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
import axios from "axios";
import React, { useEffect, useState } from "react";

const App = () => {
  const [todos, setTodos] = useState([]);
  useEffect(() => {
    const getTodos = async () => {
      try {
        const res = await axios.get("http://localhost:4000/todos");
        setTodos(res.data);
      } catch (error) {
        console.error("에러가 발생하였습니다.");
      }
    };
    getTodos();
  }, []);
  return (
    <div>
      {todos.map((todo) => {
        return <div key={todo.id}>{todo.title}</div>;
      })}
    </div>
  );
};

export default App;
```

생성한 Todos를 정상적으로 서버에서 가져와서 state가 set 한 것을 확인할 수 있다.

![](/assets/images/2024/2024-04-02-17-19-02.png)

<br>

## 2.2 POST

> post 메서드는 클라이언트에서 서버로 데이터를 보낼 때 주로 사용된다.<br>
> 일반적으로 데이터는 HTTP 요청의 본문(body)에 담겨 서버로 전송된다.

예를들어 사용자가 어떤 폼을 작성하고 제출할 때, 그 폼 데이터가 POST 요청으로 서버에 전송되어 데이터를 <span style="color:indianred">추가하거나 업데이트</span>할 수 있다.

```js
axios.post(url[, data[, config]])   // POST
```

<br>

input에 어떤 값을 넣고 버튼을 클릭했을 때 todo를 body에 담아 서버로 POST 요청을 보내어 값을 추가해보자.

```js
import axios from "axios";
import React, { useEffect, useState } from "react";

const App = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState("");

  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const res = await axios.get("http://localhost:4000/todos");
        setTodos(res.data);
      } catch (err) {
        console.err("에러가 발생하였습니다.");
      }
    };
    fetchTodos();
  }, []);

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };

  const handleSubmitButton = async (e) => {
    e.preventDefault();
    try {
      const res = await axios.post("http://localhost:4000/todos", {
        title: inputValue,
      });
      setTodos([...todos, res.data]);
      setInputValue("");
    } catch (error) {
      console.error("에러가 발생하였습니다.");
    }
  };
  return (
    <div>
      <form onSubmit={handleSubmitButton}>
        <input
          type="text"
          value={inputValue}
          onChange={handleInputChange}
        ></input>
        <button>추가하기</button>
      </form>
      {todos.map((todo) => (
        <div key={todo.id}>{todo.title}</div>
      ))}
    </div>
  );
};

export default App;
```

<br>

## 2.3 delete

DELETE는 저장되어 있는 데이터를 삭제하고자 요청을 보낼 때 사용한다.

```js
axios.delete(url[, config])  // DELETE
```

```js
import axios from "axios";
import React, { useEffect, useState } from "react";

const App = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState("");

  // 조회 함수
  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const res = await axios.get("http://localhost:4000/todos");
        setTodos(res.data);
      } catch (err) {
        console.err("에러가 발생하였습니다.");
      }
    };
    fetchTodos();
  }, []);

  // 추가 함수
  const onhandleSubmitButton = async (e) => {
    e.preventDefault();
    try {
      const res = await axios.post("http://localhost:4000/todos", {
        title: inputValue,
      });
      setTodos([...todos, res.data]);
      setInputValue("");
    } catch (error) {
      console.error("에러가 발생하였습니다.");
    }
  };

  // 삭제 함수
  const onDeleteHandleButton = async (id) => {
    try {
      await axios.delete(`http://localhost:4000/todos/${id}`);
      setTodos(
        todos.filter((item) => {
          return item.id !== id;
        })
      );
    } catch (error) {
      console.error("삭제 중 에러 발생", error);
    }
  };

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };
  return (
    <div>
      <form onSubmit={onhandleSubmitButton}>
        <input
          type="text"
          value={inputValue}
          onChange={handleInputChange}
        ></input>
        <button>추가하기</button>
      </form>
      {todos.map((todo) => (
        <div key={todo.id}>
          <span>{todo.title}</span>
          <button onClick={() => onDeleteHandleButton(todo.id)}>
            삭제하기
          </button>
        </div>
      ))}
    </div>
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
import axios from "axios";
import React, { useEffect, useState } from "react";

const App = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState("");
  const [editId, setEditId] = useState(null); // 수정 중인 항목의 ID
  const [editValue, setEditValue] = useState(""); // 수정 입력 값

  // 조회 함수
  useEffect(() => {
    const fetchTodos = async () => {
      try {
        const res = await axios.get("http://localhost:4000/todos");
        setTodos(res.data);
      } catch (err) {
        console.err("에러가 발생하였습니다.");
      }
    };
    fetchTodos();
  }, []);

  // 추가 함수
  const onhandleSubmitButton = async (e) => {
    e.preventDefault();
    try {
      const res = await axios.post("http://localhost:4000/todos", {
        title: inputValue,
      });
      setTodos([...todos, res.data]);
      setInputValue("");
    } catch (error) {
      console.error("에러가 발생하였습니다.");
    }
  };

  // 삭제 함수
  const onDeleteHandleButton = async (id) => {
    try {
      await axios.delete(`http://localhost:4000/todos/${id}`);
      setTodos(
        todos.filter((item) => {
          return item.id !== id;
        })
      );
    } catch (error) {
      console.error("삭제 중 에러 발생", error);
    }
  };

  // 수정 함수
  const onEditHandleButton = async (id) => {
    try {
      const res = await axios.put(`http://localhost:4000/todos/${id}`, {
        title: editValue,
      });
      setTodos(
        todos.map((item) =>
          item.id === id ? { ...item, title: res.data.title } : item
        )
      );
      setEditId(null); // 수정 모드 종료
      setEditValue("");
    } catch (error) {
      console.error("수정 중 에러 발생", error);
    }
  };

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };

  const handleEditInputChange = (e) => {
    setEditValue(e.target.value);
  };
  return (
    <div>
      <form onSubmit={onhandleSubmitButton}>
        <input type="text" value={inputValue} onChange={handleInputChange} />
        <button type="submit">추가하기</button>
      </form>
      {todos.map((todo) => (
        <div key={todo.id}>
          {editId === todo.id ? (
            <input
              type="text"
              value={editValue}
              onChange={handleEditInputChange}
            />
          ) : (
            <span>{todo.title}</span>
          )}
          <button onClick={() => onDeleteHandleButton(todo.id)}>
            삭제하기
          </button>
          {editId === todo.id ? (
            <button onClick={() => onEditHandleButton(todo.id)}>
              저장하기
            </button>
          ) : (
            <button
              onClick={() => {
                setEditId(todo.id);
                setEditValue(todo.title);
              }}
            >
              수정하기
            </button>
          )}
        </div>
      ))}
    </div>
  );
};

export default App;
```

<br><br>

# 3. Fetch vs axios

Fetch는 자바스크립트 내장 브라우저이기 때문에 별도의 설치 및 import를 필요로 하지 않는다.

<br>

## 3.1 자동 JSON 변환

axios는 응답을 자동으로 JSON으로 변환하지만, fetch를 사용할 때는 응답을 JSON으로 수동 변환해야한다.

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

## 3.2 에러 처리

axios는 네트워크 오류가 발생했거나 서버가 2xx 범위가 아닌 상태 코드를 반환했을 때 에러를 발생시키지만,<br>fetch는 네트워크 오류 시에만 예외를 발생시키고, HTTP 오류 상태에 대해서는 예외를 발생시키지 않는다.

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

- fetch는 개발자가 일일히 then() 안에 모든 케이스에 대한 HTTP 에러 처리를 해야한다.

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

## 3.3 요청 취소

axios는 요청 취소 기능을 제공하지만, fetch는 기본적으로 이 기능을 제공하지 않는다.

> axios

```js
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios
  .get("https://api.example.com/data", {
    cancelToken: source.token,
  })
  .catch(function (thrown) {
    if (axios.isCancel(thrown)) {
      console.log("Request canceled", thrown.message);
    }
  });

// 요청 취소
source.cancel("Operation canceled by the user.");
```

<br>

> 언제 요청 취소가 필요할까?

1. 자동완성 기능이 있는 검색 바에서 사용자가 입력을 계속 변경하면 이전 검색 요청을 취소하여 불필요한 네트워크 트래픽과 서버 부하를 줄일 수 있다.
2. 사용자가 페이지를 빠르게 넘길 때 이전 페이지의 데이터 로딩을 취소할 수 있다.
3. 사용자가 파일 업로드, 데이터 검색 등의 긴 작업을 시작한 후 작업 취소를 원할 때, 진행 중인 HTTP 요청을 취소할 수 있다.
4. 일정 시간 내에 응답이 오지 않을 경우, 요청을 취소하고 사용자에게 피드백을 제공할 수 있다.

추후 프로젝트에서 활용해보기!

<br>

## 3.4 타임아웃 설정

axios를 사용하면 요청에 대한 타임아웃을 설정할 수 있지만, fetch는 기본적으로 이 기능을 제공하지 않는다.

> axios

```js
axios
  .get("https://api.example.com/data", {
    timeout: 1000, // 1000ms 후에 타임아웃
  })
  .then((response) => {
    console.log(response);
  })
  .catch((error) => {
    console.log("Timeout or other error:", error);
  });
```

<br>

## 3.5 인터셉터

axios는 요청과 응답을 인터셉트할 수 있는 기능을 제공하여, 전역적으로 요청 또는 응답을 처리하거나 수정할 수 있도록 한다.

> axios

```js
// 요청 인터셉터 추가
axios.interceptors.request.use((request) => {
  console.log("Starting Request", request);
  return request;
});

// 응답 인터셉터 추가
axios.interceptors.response.use((response) => {
  console.log("Response:", response);
  return response;
});
```

<br>

# 4 .env를 활용한 리팩토링

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

# 5. axios interceptor 개요

## 5.1 axios interceptor 개념

Axios 라이브러리에서 제공하는 기능 중 하나로, Axios 요청 및 응답을 가로채어 변형하는 역할을 한다.

요청 헤더 수정, 인증 토큰 추가, 로그 관련 로직 삽입, 에러 핸들링 등 요청 및 응답시에 필요한 작업들을 한꺼번에 처리를 할 수 있다.

(1) 요청(request)이 처리되기 전( = http request가 서버에 전달되기 전)<br>
(2) 응답(response)의 then(=성공) 또는 catch(=실패)가 처리되기 전

![](/assets/images/2024/2024-02-18-23-06-01.png)

[출처 : https://javascript.plainenglish.io/how-to-implement-a-request-interceptor-like-axios-896a1431304a]

<br>

## 5.2 axios interceptor 사용 예제

```js
import axios from "axios";

// Axios 인스턴스 생성
const api = axios.create({
  baseURL: "https://api.example.com",
});

// 요청 인터셉터 추가
api.interceptors.request.use(
  (config) => {
    // 요청을 보내기 전에 수행할 작업
    console.log("Request sent at:", new Date().toISOString());
    // 예: 인증 토큰이 필요한 경우 헤더에 추가
    const token = localStorage.getItem("authToken");
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    // 요청 오류 처리
    console.log("Request error:", error);
    return Promise.reject(error);
  }
);

// 응답 인터셉터 추가
api.interceptors.response.use(
  (response) => {
    // 응답 데이터를 가공
    console.log("Response received at:", new Date().toISOString());
    return response;
  },
  (error) => {
    // 응답 오류 처리
    if (error.response && error.response.status === 401) {
      console.log("Unauthorized access:", error);
      // 예: 로그인 페이지로 리디렉트
    }
    return Promise.reject(error);
  }
);

export default api;
```

<br><br>

# 6. axios instance 사용 예제

## 6.1 instance 만들기, baseURL 설정하기

지금까지 custom 설정이 전혀 되어있지 않은 axios를 통해 데이터 통신을 했다.<br>
이 axios를 인스턴스(instance)라고 한다.

axios instance는 단순히 import해서 가져온 객체일 뿐이다.

```js
import axios from "axios";

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
  timeout: 1000, // 모든 요청에 대해 1000ms 후 타임아웃
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

## 6.2 request, response에 적용하기

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

## 6.3 실패 시켜보기

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

<br><br>

# 7. 통신 연습

## 7.1 기초 세팅

먼저 json-server와 axios를 설치하자

```
yarn add axios
yarn global add json-server
```

<br>

그리고 db.json 파일을 생성하자

```json
{
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "johndoe@example.com"
    },
    {
      "id": 2,
      "name": "Jane Doe",
      "email": "janedoe@example.com"
    }
  ],
  "posts": [
    {
      "id": 1,
      "userId": 1,
      "title": "John's first post",
      "body": "This is the content of John's first post."
    },
    {
      "id": 2,
      "userId": 1,
      "title": "John's second post",
      "body": "This is the content of John's second post."
    },
    {
      "id": 3,
      "userId": 2,
      "title": "Jane's first post",
      "body": "This is the content of Jane's first post."
    }
  ]
}
```

<br>

json-server을 실행하면 된다.

```
npx json-server db.json --port [원하는 포트]
```

<br>

다음 db.json 파일을 이용하여 백엔드를 세팅하자

```json
{
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "johndoe@example.com"
    },
    {
      "id": 2,
      "name": "Jane Doe",
      "email": "janedoe@example.com"
    }
  ],
  "posts": [
    {
      "id": 1,
      "userId": 1,
      "title": "John's first post",
      "body": "This is the content of John's first post."
    },
    {
      "id": 2,
      "userId": 1,
      "title": "John's second post",
      "body": "This is the content of John's second post."
    },
    {
      "id": 3,
      "userId": 2,
      "title": "Jane's first post",
      "body": "This is the content of Jane's first post."
    }
  ]
}
```

<br>

## 7.2 연습 문제

> 본격적으로 통신 연습을 진행해보자.

먼저, App.jsx의 내용은 변경하지 않고 그대로 붙여넣자.

```js
// src > App.jsx
import React, { useEffect, useState } from "react";
import api from "./api";

function App() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await api.get("/posts");
      } catch (error) {
        console.error("There was an error!", error);
      }
    };

    fetchPosts();
  }, []);

  return (
    <div>
      <h1>Posts</h1>
      {posts.map((post) => (
        <div key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.body}</p>
        </div>
      ))}
    </div>
  );
}

export default App;
```

<br>

그 다음, api.js 파일을 src 밑에 생성한 후 내용을 채워보자.

아래의 조건을 만족해야한다.

> 1. 기초 URL이 json-server를 가리켜야한다.
> 2. 요청시 항상 console.log("요청합니다."); 를 출력해야한다.
> 3. 응답시 항상 console.log("응답입니다."); 를 출력해야한다.

<br>

아래의 코드를 베이스로 조건을 충족시키면 된다.

```js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create();

// 요청 인터셉터 추가
api.interceptors.request.use();

// 응답 인터셉터 추가
api.interceptors.response.use();

export default api;
```

<br>

## 7.3 문제 풀이

처음에 내가 작성한 코드는 아래와 같다.

```js
// api.js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create({
  baseURL: "http://localhost:3001",
});

// 요청 인터셉터 추가
api.interceptors.request.use(console.log("요청합니다."));

// 응답 인터셉터 추가
api.interceptors.response.use(console.log("응답합니다."));

export default api;
```

<br>

초기 작성한 코드는 요청 인터셉터와 응답 인터셉터의 위치를 바꿔주었을 때, 응답이 먼저 되고 그 후 요청이 되는 문제가 발생했다.

```js
// 응답 인터셉터 추가
api.interceptors.response.use(console.log("응답합니다."));

// 요청 인터셉터 추가
api.interceptors.request.use(console.log("요청합니다."));
```

![](/assets/images/2024/2024-02-20-01-06-58.png)

<br>

interceptors.request.use 및 interceptors.response.use에서 사용하는 함수는 비동기 함수여야한다.<br> 따라서 console.log를 직접 전달하는 대신 함수를 전달하는 방식으로 바꿔주었다.

```js
import axios from "axios";

// TODO: Axios 인스턴스를 생성합니다. App.jsx
const api = axios.create({
  baseURL: "http://localhost:3001",
});

// 응답 인터셉터 추가
api.interceptors.response.use(function (response) {
  console.log("응답합니다.");
  return response;
});

// 요청 인터셉터 추가
api.interceptors.request.use(function (request) {
  console.log("요청합니다");
  return request;
});

export default api;
```

이젠 응답 인터셉터와 요청 인터셉터의 위치를 바꿔줘도 정상적으로 응답 먼저 되는 것을 확인할 수 있다!

<br>
