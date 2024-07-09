---
title: "[JS] JavaScript 비동기 프로그래밍 정리: fetch, axios, Promise, async/await, then/catch, try/catch"
categories: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
nav: "counts"
---

<br>

> 비동기를 다시 공부하다보니 `fetch`, `axios`, `promise`, `async와 await`, `then~catch`, `try~catch`와 같은 용어들이 혼동이 와서 이를 정리하는 글이다.

먼저 내가 가장 도움이 된 블로그를 소개하겠다! [[Javascript] 비동기, Promise, async, await 확실하게 이해하기](https://springfall.cc/article/2022-11/easy-promise-async-await)

<br><br>

# 0. 동기와 비동기

먼저 동기와 비동기의 개념부터 짚고 가자.

> 동기와 비동기의 특징

- 동기
  - 동시에 여러 작업을 수행할 수 없음
  - 코드의 실행 흐름을 예측하기 쉬움
- 비동기
  - 동시에 여러 작업을 수행할 수 있음
  - 코드의 실행 흐름을 예측하기 어려움 -> 무엇이 먼저 완료될 지 보장 x

<br>

> 예시를 통해 알아보는 동기와 비동기

- 모든 생선이 손질된 이후 요리를 시작할 수 있다고 가정

  - 동기는 순차적으로 생선 손질을 함 -> 시간이 오래 걸리기는 하지만, 어떤 순서로 진행할지 명확함
  - 비동기는 생선 손질을 동시에 시작할 수 있으므로 상당히 효율적으로 보임 -> but 어떤 생선이 언제 손질될지 예측하기가 어려움
  - 따라서 비동기는 모든 생선 손질이 끝나고 요리를 시작해야 하는 코드는 도대체 어디에 작성해야하는지 의문이 생김(아래와 같이 끔찍한 코드가 나오게 된다.)<br><br>

    ```js
    console.log("모두에게 일을 시켜보자! (각 팀원은 1~2초의 소요시간이다.)");

        let aFinished = false;
        let bFinished = false;
        let cFinished = false;

        setTimeout(
        () => {
        console.log("A: 일을 마쳤습니다!");
        aFinished = true;
        if (aFinished && bFinished && cFinished) {
        console.log("일을 다 마쳤으니 이제 요리를 시작하자!");
        }
        },
        Math.random() \* 1000 + 1000,
        );

        setTimeout(
        () => {
        console.log("B: 일을 마쳤습니다!");
        bFinished = true;
        if (aFinished && bFinished && cFinished) {
        console.log("일을 다 마쳤으니 이제 요리를 시작하자!");
        }
        },
        Math.random() \* 1000 + 1000,
        );

        setTimeout(
        () => {
        console.log("C: 일을 마쳤습니다!");
        cFinished = true;
        if (aFinished && bFinished && cFinished) {
        console.log("일을 다 마쳤으니 이제 요리를 시작하자!");
        }
        },
        Math.random() \* 1000 + 1000,
        );

        console.log("일은 전부 시켜놓았다!");

    ```

  1.  위 코드는 비동기적으로 실행되기 때문에 A, B, C의 <span style="color:indianred">작업 순서를 보장할 수 없다.</span>
  2.  또한 비동기 작업은 코드의 가독성이 떨어지고, 유지보수가 어려워지는 <span style="color:indianred">콜백지옥이 발생</span>한다.

<br><br>

# 1. 비동기 프로그래밍 요약

| 개념            | 설명                                                                                  |
| --------------- | ------------------------------------------------------------------------------------- |
| **fetch**       | 네트워크 요청을 수행하는 내장 API.                                                    |
| **axios**       | 네트워크 요청을 수행하는 라이브러리로, 추가 기능(인터셉터, 자동 JSON 변환 등)을 제공. |
| **Promise**     | 비동기 작업의 결과를 나타내는 객체.                                                   |
| **async/await** | `Promise`를 쉽게 처리할 수 있도록 도와주는 최신 문법.                                 |
| **then/catch**  | `Promise`의 결과를 처리하는 메소드.                                                   |
| **try/catch**   | 에러 처리를 위한 구문으로, `async/await`와 함께 사용.                                 |

### 공통점과 차이점

| 항목                  | fetch                | axios                                   | Promise                             | async/await                         | then/catch                          | try/catch                              |
| --------------------- | -------------------- | --------------------------------------- | ----------------------------------- | ----------------------------------- | ----------------------------------- | -------------------------------------- |
| **비동기 프로그래밍** | 네트워크 요청을 처리 | 네트워크 요청을 처리                    | 비동기 작업을 나타내는 객체         | `Promise`를 쉽게 처리               | `Promise` 결과 처리                 | 에러 처리                              |
| **Promise 반환**      | 항상 `Promise` 반환  | 항상 `Promise` 반환                     | `Promise` 객체 자체                 | `Promise` 기반 문법                 | `Promise` 객체 기반                 | `Promise` 및 기타 에러 처리            |
| **추가 기능**         | 없음                 | 인터셉터, 자동 JSON 변환 등 다양한 기능 | 비동기 작업의 완료 또는 실패 나타냄 | `Promise`를 동기 코드처럼 작성 가능 | `Promise`의 `then`과 `catch` 메소드 | `try` 블록 내 에러를 `catch` 블록 처리 |

<br><br>

# 2. fetch vs axios

<br>

> 공통점과 차이점

- 공통점: 프로미스 기반의 fetch api와 axios는 항상 프로미스 객체를 반환한다.
- 차이점: Fetch와 Axios는 HTTP 요청을 보내는 방법에 차이가 있다

<br>

> fetch

```js
fetch("https://api.example.com/data")
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((error) => console.error("Error:", error));
```

<br>

> axios

```js
axios
  .get("https://api.example.com/data")
  .then((res) => console.log(res.data))
  .catch((error) => console.error("Error:", error));
```

<br>

> 관련 포스팅

- [fetch()함수로 HTTP 요청하기](<https://mynamesieun.github.io/javascript/fetch()%ED%95%A8%EC%88%98%EB%A1%9C-HTTP-%EC%9A%94%EC%B2%AD%ED%95%98%EA%B8%B0/>)
- [비동기 통신 - axios와 interceptor](https://mynamesieun.github.io/react/%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-axios%EC%99%80-interceptor/)

<br><br>

# 3. Promise vs async와 await

## 3.1 Promise

- Promise는 비동기 작업의 완료 또는 실패를 나타내는 객체이다.
- 비동기 처리 결과(resolve(성공), reject(실패))와, 진행 상태(pending(대기), filfilled(이행), rejected(실패)) 상태를 갖는다.
- `.then()`과 `.catch()` 메서드를 사용하여 비동기 작업의 결과를 처리할 수 있다.

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
  }, 1000);
});

myPromise
  .then((value) => console.log(value))
  .catch((error) => console.error(error));
```

<br>

> 관련 포스팅

- [프로미스 객체와 메서드](https://mynamesieun.github.io/javascript/%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4-%EA%B0%9D%EC%B2%B4%EC%99%80-%EB%A9%94%EC%84%9C%EB%93%9C/)

<br>

# 3.2 async와 await

- Promise를 더 쉽게 사용할 수 있도록 하는 문법적 설탕(syntactic sugar)이다.
- async 함수의 리턴 값은 무조건 Promise이다.
- async 함수 내에서 await 키워드를 사용하면, Promise의 결과를 기다린 후 다음 코드를 실행할 수 있어 비동기 코드를 동기 코드처럼 보이게 만들어 가독성을 높일 수 있다.

```js
async function fetchData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}

fetchData();
```

<br>

> 프로미스를 사용하면 콜백 지옥이 발생한다는 문제가 있지만, async와 awite를 사용하면 아래와 같은 이점이 있다.

1. 프로미스를 간결하게 만들어 줌
2. 에러 핸들링에 유리
3. 에러 위치를 찾기 쉬움

<br>

> 관련 포스팅

- [async와 await](https://mynamesieun.github.io/javascript/async%EC%99%80-await/)

<br><br>

# 4. then~catch vs try~catch

> Then/Catch와 Try/Catch는 비동기 작업의 결과를 처리하는 방식에 차이가 있다.

## 4.1 then~catch

- Promise 기반의 비동기 작업을 처리할 때 사용한다.
- `.then()`으로 <span style="color:indianred">성공</span>했을 때 실행할 콜백을 등록하고, `.catch()`로 <span style="color:indianred">실패</span> 했을 때 콜백을 등록한다.
- 실제 작업이 끝난 다음 후속 조치를 하는 느낌에 가깝다.

```js
myPromise
  .then((value) => console.log(value))
  .catch((error) => console.error(error));
```

<br>

## 4.2 try~catch

- async/await를 사용할 때 비동기 작업에서 발생하는 에러를 처리하기 위해 사용한다.
- try 블록 안에서 await를 사용하여 시도할 비동기 작업을 적고, 에러가 발생하면 catch 블록이 실행되어 에러를 처리한다.
- 실제 작업이 끝나는 걸 기다린 다음, 다음 코드를 수행하는 느낌에 가깝다.

```js
async function fetchData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error:", error);
  }
}

fetchData();
```

<br><br>

# 5. API로부터 받아온 데이터를 렌더링하기

API로부터 받아온 데이터를 렌더링하기 위해, 주로 `useState`와 `useEffect` 훅을 사용하여 데이터를 가져오고 상태에 저장한 다음, 데이터를 컴포넌트에 렌더링한다.

## 5.1 fetch()

```jsx
import { useEffect, useState } from "react";

function App() {
  const [data, setData] = useState([]); // 1단계 : 상태 변수 생성

  useEffect(() => {
    // 2단계 : 데이터 가져오기
    fetch("http://localhost:9999/topics")
      .then((res) => res.json())
      .then((result) => {
        setData(result); // 3단계 : 데이터 저장
      });
  }, []);

  return (
    <div>
      {/* 4단계 : 데이터 렌더링 */}
      {data.map((item) => (
        <div key={item.id}>
          <h2>{item.title}</h2>
          <p>{item.body}</p>
        </div>
      ))}
    </div>
  );
}

export default App;
```

<br>

> 작성한 함수가 아닌 곳에서 데이터를 필요로 할 경우

```js
useEffect(() => {
  async function fetchData() {
    const res = await fetch("API URL");
    const result = await res.json();
    /* 1. 데이터를 return 하고 */
    return result;
  }
  /* 2. then을 통해 response를 넘긴다. */
  fetchData().then((res) => setDocs(res));
}, []);
```

<br>

## 5.2 axios

```js
import axios from "axios";
import { useState, useEffect } from "react";

const App = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    axios.get("http://localhost:4000/topics").then((res) => {
      setData(res.data);
    });
  }, []);

  return (
    <div>
      {data.map((topic) => (
        <div key={topic.id}>
          <div>{topic.title}</div>
          <div>{topic.body}</div>
        </div>
      ))}
    </div>
  );
};

export default App;
```

<br>

## 5.3 async/await

> 문법적 설탕 async/await 사용

비동기 코드를 동기적으로 보이게 하기 위해 변수 `res`에 저장하여 다음 코드에 사용할 수 있도록 한다.

```js
import axios from "axios";
import { useState, useEffect } from "react";

const App = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    const getTopics = async () => {
      try {
        const res = await axios.get("http://localhost:4000/topics");
        setData(res.data);
      } catch (error) {
        console.error("데이터를 불러오는 중 에러가 발생했습니다.");
      }
    };
    getTopics();
  }, []);

  return (
    <div>
      {data.map((topic) => (
        <div key={topic.id}>
          <div>{topic.title}</div>
          <div>{topic.body}</div>
        </div>
      ))}
    </div>
  );
};

export default App;
```

<br>

> 작성한 함수가 아닌 곳에서 데이터를 필요로 할 경우

```js
useEffect(() => {
  async function fetchData() {
    const res = await axios.get("API URL");
    const result = res.data;
    /* 1. 데이터를 return 하고 */
    return result;
  }
  /* 2. then을 통해 response를 넘긴다. */
  fetchData().then((res) => setDocs(res));
}, []);
```

<br>

# 참조

[React API 호출 - Fetch & Axios](https://velog.io/@art11010/React-API-%ED%98%B8%EC%B6%9C#fetch-api%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-api-%ED%98%B8%EC%B6%9C)

<br>
