---
title: "[React] React Qurey"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. React Query 개요

## 1.1 기존 미들웨어의 한계

> 서버와의 API 통신과 비동기 데이터 관리에 Redux, Redux-thunk, Redux-Saga 등 사용했지만, 아래와 같은 문제가 있었다.

**① Boilerplate 코드**: 코드량이 너무 많다.

**② 복잡성**: 비동기 데이터 패칭과 데이터 캐싱하는 로직이 복잡하다.

**③ Redux는 API 통신 및 비동기 상태 관리 라이브러리가 아님!**

- Redux를 사용하여 비동기 데이터를 관리하기 위해서는 관련된 코드를 하나부터 열까지 개발자가 결정하고 구현해야 한다.
- EX: API의 로딩 여부를 `Boolean`을 사용해서 관리하는 경우, `IDLE | LOADING | SUCCESS | ERROR` 등 상태를 세분화하여 관리하는 경우

➡️ 따라서 위 문제를 해결할 React Query가 등장하게 됐다.

<br>

## 1.2 React Query의 등장

> 리액트 쿼리는 <span style="color:indianred">서버 상태 관리</span>를 쉽게 하도록 도와주는 라이브러리이다. React Query의 v4 부터 라이브러리명이 Tanstack Query로 변경되었다!

서버 상태 관리를 쉽게 한다는 게 무슨 뜻일까?

1. fetching: 서버에서 데이터 받아오기
2. caching: 서버에서 받아온 데이터를 따로 보관해서 동일한 데이터가 단 시간 내에 다시 필요할 시 서버 요청없이 보관된 데이터에서 꺼내쓰기
3. synchronizing: 서버상의 데이터와 보관 중인 캐시 데이터(서버상태)를 동일하게 만들기 (동기화)
4. updating : 서버 데이터 변경 용이 (mutation & invalidateQueries)

<br>

➡️ 즉, 리액트 쿼리는 쿼리데이터 패칭, 캐싱, 동기화를 쉽게 관리할 수 있게 해주며, 이를 통해 UI를 자동으로 업데이트할 수 있다. 이로써 서버 상태와 클라이언트 상태의 동기화를 용이하게 해준다.

➡️ 또한, 쿼리 상태(로딩, 에러, 데이터)를 쉽게 관리할 수 있게 해주며, 이를 통해 UI를 자동으로 업데이트할 수 있다. (=> 데이터 일관성 유지)

<br>

## 1.3 핵심 기능

1. **자동 데이터 캐싱**: 요청된 데이터는 자동으로 캐싱되며, 동일한 요청은 캐시에서 빠르게 로드된다.
2. **자동 리패치**: 데이터가 변경되거나 페이지가 포커스될 때 자동으로 데이터를 다시 가져올 수 있다.
3. **백그라운드 데이터 갱신**: 애플리케이션은 백그라운드에서 자동으로 최신 데이터를 fetching하여 사용자에게 최신 상태를 제공할 수 있다. ex) 크롬 탭 전환 시, 특정 버튼 클릭이나 마우스를 올렸을 시 등
4. **페이지네이션과 인피니트 스크롤**: 페이지네이션과 인피니트 스크롤 구현을 위한 편리한 훅(**`useInfiniteQuery`**)을 제공한다.
5. **데이터 동기화**: 데이터를 생성, 업데이트, 삭제하는 동작 후에 관련된 쿼리를 자동으로 무효화하고 최신 상태로 업데이트할 수 있다.

<br><br>

# 2. Server-Side State vs Client-Side State

## 2.1 서버 사이드 상태(Server-Side State)

> 서버에 저장되고 관리되는 데이터를 말한다. 데이터베이스, 파일 시스템, 또는 서버의 메모리 등에 저장될 수 있다.

- 동기화와 일관성: 클라이언트에서 서버로 데이터를 요청하거나 업데이트하면서 발생하는 네트워크 지연과 <span style="color:indianred">데이터 동기화</span>에 대한 고려가 필요하다.
- 지속성: 데이터가 서버에 저장되므로, 클라이언트의 상태가 초기화되거나 변경되어도 영구적으로 유지된다.
- 예시: 데이터베이스에 저장된 사용자 정보, 게시글 목록 등

<br>

## 2.2 클라이언트 사이드 상태(Client-Side State)

> 사용자의 브라우저 내에서 저장되고 관리되는 상태를 말한다. 리액트 컴포넌트의 `useState` 또는 상태 관리 라이브러리(ex) Redux, Recoil)를 통해 관리된다.

- 로컬 데이터: 주로 UI 컴포넌트의 상태, 사용자 입력, 토글 상태와 같이 로컬에서만 필요한 데이터를 관리한다.
- 성능: 네트워크 지연 없이 빠르게 상태를 변경하고 반응할 수 있다.
- 수명 주기: 페이지를 새로고침하거나 애플리케이션을 종료할 때 초기화되는 경향이 있다.
- 예시: 사용자의 입력 데이터, UI 상태(예: 보이기/숨기기), 현재 페이지네이션 위치

<br><br>

# 3. 주요개념

## 3.1 React Query 의 swr 전략

> swr(stale-while-revalidate) 전략이란?

- 신규 데이터가 도착하는 동안 일단 기존 캐싱된 데이터를 사용하도록 하는 전략이다.
- 즉, 썩은(오래된) 데이터를 새로운 데이터가 도착하기 전까지 사용하는 것이다.

<br>

## 3.2 QueryClientProvider

- QueryClientProvider의 역할: React Query의 컨텍스트를 설정하여 애플리케이션 전역에 걸쳐 데이터 패칭 및 상태 관리를 가능하게 한다.
- Context API 사용: React Query는 Context API를 활용하여 QueryClient를 공급하고, 이를 통해 데이터와 상태를 중앙에서 관리한다.
- 자식 컴포넌트의 데이터 접근: QueryClientProvider의 자식 컴포넌트들은 QueryClient를 통해 캐시 데이터에 접근할 수 있다.
- 전역 상태: QueryClient를 통해 캐시 데이터가 전역적으로 관리되므로, 페이지 컴포넌트 외부에서도 데이터가 일관되게 유지된다.

```jsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
// React Query에서 모든 쿼리와 캐시된 데이터는 QueryClient의 메모리 내에 보관
// QueryClient는 React Query의 핵심 객체로, 쿼리와 캐시의 상태를 관리
const queryClient = new QueryClinet();

const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <Router />
    </QueryClientProvider>
  );
};
```

<br>

## 3.3 React Query의 데이터 흐름

> "헌거 먼저, 리렌더링되면서 새거로 교체" 한다는 것을 기억하자!

![](/assets/images/2024/2024-07-21-15-03-50.png)

<br>

## 3.5 React Query의 Lifecycle

> 하나의 쿼리 인스턴스(하나의 query key)마다 같은 Lifecycle을 가진다.

fetching을 해서 가져온 "직후" 데이터는 늘 "fresh"

```jsx
const data = useQuery(["abc"], () => {});
```

![](/assets/images/2024/2024-07-22-11-50-05.png)

<br>

> ① fresh(신선한)

- "Fresh" 상태는 데이터가 최신 상태임을 의미한다.
- 즉, 최근에 가져온(fetch) 데이터이며 staleTime이 아직 경과하지 않았다.
- 이 상태에서는 추가적인 데이터 요청이 발생해도 리액트 쿼리가 데이터를 재요청하지 않고 현재 캐시된 값을 사용한다.

<br>

> ② stale(썩은)

- "Stale" 상태는 데이터가 더 이상 최신 상태가 아니며 <span style="color:indianred">재검증(revalidation)</span>이 필요할 수 있는 상태를 의미한다.
- staleTime이 경과하면 데이터는 자동으로 "stale" 상태가 된다.
- 이 상태에서 쿼리가 다시 실행되면, 리액트 쿼리는 자동으로 백그라운드에서 데이터를 새로고침하여 최신 상태로 유지하려고 시도한다. → query key
  <br><br>
- staleTime 과 stale/fresh 의 관계
  - `staleTime > 0`인 경우, 가져온 데이터는 그 시간 만큼 fresh한 것으로 간주(= fresh data)
  - `staleTime = 0`인 경우, 가져온 데이터는 즉시 stale한 것으로 간주(= stale data)

<br>

> ③ staleTime

- 특정 쿼리에 대한 데이터가 "싱싱함(fresh)" 상태로 유지되는 시간을 정의하며, 이 시간이 지나면 데이터는 "썩음(stale)"으로 간주된다.
- 즉, `staleTime` 설정 시간 동안에는 쿼리를 다시 실행해도 서버로부터 데이터를 재요청(fetch)하지 않으며, 캐시된 데이터를 사용한다.
- staleTime의 기본값은 0ms이기 때문에 쿼리가 성공적으로 해결되면 데이터가 즉시 'stale'(오래된 상태)로 표시된다. -> 최신 상태 유지 가능
- 예: staleTime이 5분(300,000ms)으로 설정되어 있다면, 데이터는 5분 동안 새로고침되지 않으며 이 기간 동안 여러 번의 요청에서도 동일한 캐시된 데이터를 재사용한다.

<br>

> ④ inActive

- 해당 데이터를 사용하는 컴포넌트가 더 이상 마운트되지 않거나 해당 쿼리를 구독하는 컴포넌트가 없는 상태를 의미한다.
- 즉, inActive 상태는 특정 쿼리에 대한 모든 옵저버(observer)가 제거되었을 때 발생한다.
- 이 상태에서는 데이터가 캐시에서 자동으로 삭제되기 전까지 일정 기간 동안 유지된다.
- inactive에서 fresh가 다시 될 수 있다.(컴포넌트 재마운트, 데이터 재요청, Refetch 함수 호출)

<br>

## 3.6 staleTime vs cacheTime

> staleTime

| 항목          | 내용                                                                                                                                                                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **뜻**        | 유통 기한                                                                                                                                                                                                                                                                                        |
| **설명**      | - 얼마의 시간이 흐른 뒤에 stale 취급할 건지를 나타낸다.(default: 0)<br> - 이 시간 동안에는 쿼리를 다시 실행해도 서버로부터 데이터를 재요청하지 않고, 캐시된 데이터를 사용한다.                                                                                                                   |
| **용도**      | 빈번한 데이터 업데이트가 필요하지 않은 경우, 네트워크 요청을 줄여 성능을 향상할 때 유용하다.                                                                                                                                                                                                     |
| **동작 방식** | `staleTime`이 지나면 데이터는 "오래됨(stale)"으로 간주되고, 해당 쿼리가 다시 호출되면 React-Query는 새로운 데이터를 패칭한다.                                                                                                                                                                    |
| **예**        | - cacheTime을 5분(300,000ms)으로 설정되어 있을 때, 사용자가 포스트를 읽고 나서 다른 페이지로 이동한 후 5분 이내에 포스트 목록으로 돌아오면 캐시된 데이터가 즉시 표시된다.<br> - 사용자가 5분 이후에 돌아온다면 캐시된 데이터는 이미 제거되었기 때문에, 새로운 데이터를 서버로부터 가져와야 한다. |

<br>

> cacheTime

| 항목          | 내용                                                                                                                                                                                                                                           |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **뜻**        | 캐시 지속 시간, 사용하지 않는 데이터를 언제까지 유지할 것인가 (사용하지 않는 데이터 보관이 핵심)                                                                                                                                               |
| **설명**      | - 쿼리 데이터가 캐시에서 제거되기 전까지 유지되는 최대 시간을 설정한다.(default: 5분, cacheTime 0되면 삭제처리)<br> - 즉 inactive 된 이후로 데이터를 캐시에 유지하는 시간이며, 쿼리가 언마운트된 후에도 이 시간 동안 캐시된 데이터가 유지된다. |
| **용도**      | 사용자가 빈번하게 동일한 데이터를 요청하는 경우, 캐시를 통해 즉각적인 응답을 제공하여 사용자 경험을 개선하고자 할 때 사용한다.                                                                                                                 |
| **동작 방식** | - `cacheTime` 동안에는 쿼리 데이터가 캐시에 남아 있어서 빠르게 재사용할 수 있다.<br> - 이 시간이 지나면 캐시된 데이터는 삭제되어, 다음 번에 해당 데이터가 필요할 때 다시 패칭해야 한다.                                                        |

<br>

> 정리하자면,

- **`staleTime`**은 데이터가 얼마나 오랫동안 "신선"으로 간주되는지를 결정하고, **`cacheTime`**은 캐시된 데이터가 메모리에 얼마나 오래 유지되는지를 결정.

- **`staleTime`**은 데이터가 캐시에 남아 있는 동안 자동 re-fetching의 필요성을 제어하는 반면, **`cacheTime`**은 캐시에서 데이터가 완전히 제거되기까지의 시간을 관리.

<br>

## 3.7 revalidate(재검증) vs invalidateQueries(Query 무효화)

> 데이터의 stale함, fresh함과 관련된 두 가지 다른 개념이다.

- revalidate(재검증)
  - `Revalidate`는 캐시된 데이터가 'stale'(오래된) 상태가 되었을 때, 이를 자동으로 감지하고 새로운 데이터를 가져오는 프로세스이다.
  - React Query는 `staleTime`이 지나거나 다른 특정 이벤트(예: 브라우저 창 포커스 재획득)가 발생했을 때 데이터를 재검증한다.
  - 이 과정은 일반적으로 백그라운드에서 이루어지며, 사용자는 데이터가 최신 상태로 유지되고 있음을 자동으로 알 수 있다.

<br>

- invalidateQueries(Query 무효화)
  - 서버의 데이터는 언제든지 변경될 수 있기 때문에, 가져온 Query가 최신 상태가 아닐 수 있다. 이런 경우, 기존의 Query를 무효화한 후 최신 데이터를 다시 가져오는 작업이 필요하며, 이 과정을 React Query에서는 자동으로 처리해준다.
  - invalidateQueries 메소드는 특정 쿼리 또는 쿼리 그룹을 명시적으로 'stale' 상태로 만든다.
  - 이는 React Query에게 해당 쿼리의 데이터가 더 이상 유효하지 않으며, 가능한 한 빨리 재검증해야 한다는 것을 알린다.
  - 사용자가 데이터를 수정하는 작업을 수행한 후, 관련 쿼리를 'invalidate'함으로써 최신 데이터로의 자동 업데이트를 트리거할 수 있다.

<br><br>

# 4. useQuery 사용하여 데이터 조회하기

## 4.1 기본 기능

> useQuery 훅

- React-Query의 `useQuery` 훅은 서버로부터 데이터를 비동기적으로 fetching하고, 그 결과를 캐싱하여 관리하는 메커니즘을 제공한다.
- 즉, 데이터에 대한 요청을 의미하며 주로 조회하는 데 사용된다. 데이터를 가져오는 작업과 비슷하다. (예: axios.get)

> 캐싱

- 패칭한 데이터를 자동으로 캐싱한다. 캐싱된 데이터는 "동일한 쿼리 키✨"로 `useQuery`가 다시 호출될 때 재사용될 수 있어, 불필요한 네트워크 요청을 줄이고 애플리케이션의 성능을 향상시킨다.
- 다른 컴포넌트에서도 `useQuery(['todos'], fetchTodos)`를 사용하면, 동일한 queryKey로 인해 데이터를 재사용하고, 캐시된 데이터를 가져온다.

<br>

## 4.2 사용 예시

React Query의 useQuery 훅은 데이터 로딩, 오류 처리, 리패칭 등 많은 기능이 내장되어 있으므로, 데이터를 패칭하기 위해 별도의 useEffect를 사용할 필요가 없다.

```jsx
const data = useQuery(["abc"], () => {});
```

```jsx
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

// Axios 인스턴스를 사용해 서버에서 Todo 리스트를 가져옴
export const fetchTodos = async () => {
  const { data } = await axios.get("http://localhost:5000/todos");
  // 함수가 호출되면, 응답으로 받은 데이터(여기서는 Todo 리스트)를 반환
  return data;
};

function TodoList() {
  // useQuery 훅을 사용하여 Todo 리스트를 패칭
  // 첫 번째 인자(queryKey)는 쿼리의 고유 키(['todos']) 배열이며, 이 키를 통해 캐싱과 재사용을 관리함
  // 두 번째 인자(queryFn)는 Todo 리스트를 패칭하는 비동기 함수임
  const {
    data: todos,
    error,
    isLoading,
  } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  // 데이터를 패칭하는 동안 로딩 상태를 표시
  if (isLoading) return <div>로딩 중...</div>;

  // 에러가 발생했을 경우, 에러 메시지를 표시
  if (error) return <div>에러 발생: {error.message}</div>;

  // 패칭이 성공적으로 완료되면, Todo 리스트를 렌더링
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}

export default TodoList;
```

<br>

## 4.3 queryKey와 queryFn

useQuery는 두 가지 인자를 받는다.

| 항목                     | 내용                                                                                                                                                                                             |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **쿼리 키(`queryKey`)**  | - 쿼리를 식별하는 고유한 키이다. 동일한 queryKey를 가진 쿼리는 동일한 데이터로 간주되어 캐싱, 리패칭 등의 동작이 공유된다. <br>- 배열로 지정해줘야하며, 첫 번째 요소는 일반적으로 문자열이 된다. |
| **패칭 함수(`queryFn`)** | - 쿼리키를 기반으로 서버로부터 데이터를 실제로 가져오는 비동기 함수이다. <br>- 이 함수는 `useQuery`에 두 번째 인자로 전달된다.                                                                   |

💡 useQuery는 queryKey를 기반으로 쿼리 캐싱을 관리하는 것이 핵심이다.

<br>

## 4.4 useQuery 주요 리턴 데이터

### 4.4.1 주요 리턴 데이터

> 반환 데이터를 더 자세히 알고 싶다면 [[공식문서↗️]](https://tanstack.com/query/v5/docs/framework/react/reference/useQuery)를 참고하자

```jsx
const {
  data,
  error,
  status,
  fetchStatus,
  isLoading,
  isFetching,
  isError,
  refetch,
  // ...
} = useQuery({
  queryKey: ["super-heroes"],
  queryFn: getAllSuperHero,
});
```

| **속성**        | **설명**                                                                                                 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| **data**        | 쿼리가 성공적으로 완료되면 반환되는 데이터이다. 데이터의 형태는 `queryFn`이 반환하는 값에 따라 달라진다. |
| **error**       | 쿼리 실행 중에 발생한 에러 객체이다. 오류가 발생하지 않으면 `undefined`이다.                             |
| **status**      | 쿼리의 현재 상태를 나타내며, `'loading'`, `'error'`, `'success'` 중 하나의 값을 가진다.                  |
| **fetchStatus** | 쿼리의 데이터 패칭 상태를 나타내며, `'fetching'`, `'paused'`, `'idle'` 중 하나의 값을 가진다.            |
| **isLoading**   | 캐싱 된 데이터가 없을 때 즉, 처음 실행된 쿼리일 때 로딩 여부에 따라 true/false로 반환된다.               |
| **isFetching**  | 캐싱 된 데이터가 있더라도 쿼리가 실행되면 로딩 여부에 따라 true/false로 반환된다.                        |
| **isSuccess**   | 쿼리 요청이 성공하면 true이다.                                                                           |
| **isError**     | 쿼리 실행 중에 오류가 발생한 경우 true이다.                                                              |
| **refetch**     | 쿼리를 수동으로 다시 실행할 수 있는 함수이다. `refetch()`를 호출하면 쿼리가 다시 실행된다.               |

<br>

### 4.4.2 isLoading vs isFetching

isLoading은 데이터가 처음 로드될 때만 true가 되는 반면, isFetching은 데이터가 처음 로드될 때뿐만 아니라 데이터가 업데이트되거나 재요청될 때도 true가 된다.

```jsx
function App() {
  const { isLoading, data, isError, isFetching } = useQuery('todos', getTodos);

  if (isFetching) {
    return <div>로딩중..!</div>;
  }

  if (isError) {
    return <div>에러</div>;
  }

  return <div>{data.map()~~~}</div>;
}
```

<br>

### 4.4.3 status vs fetchStatus

- status는 data가 있는지 없는지에 대한 상태를 의미한다.
- fetchStatus는 쿼리 즉, queryFn 요청이 진행 중인지 아닌지에 대한 상태를 의미한다.

<br>

## 4.5 useQuery 주요 옵션

`useQuery`의 인자로 구성 옵션을 제공할 수 있다. 이 옵션들을 통해 쿼리의 캐싱 동작을 세밀하게 제어할 수 있다.

### 4.5.1 staleTime

- 기본값 : staleTime: 0 (fetch 후에 바로 stale이 됨)
- 설명: 쿼리 데이터가 stale(썩은) 것으로 간주되기까지의 시간을 밀리초 단위로 설정한다. 이 시간 동안에는 쿼리 인스턴스가 새롭게 mount 되어도 데이터를 다시 패칭하지 않는다.
- 특징: staleTime이 설정된 기간 동안 데이터는 "fresh" 상태로 유지된다.
- 예: staleTime이 5분(300,000ms)으로 설정되어 있으면, 쿼리 데이터는 5분 동안 신선하다고 간주되며, 이 시간 동안 쿼리를 다시 요청해도 데이터가 재패칭되지 않는다.

```jsx
const { data, error, isLoading } = useQuery(
  "todos",
  fetchTodos,
  { staleTime: 10000 } // 데이터가 10초 동안 신선하게 유지됨
);
```

⚠️ 최신 데이터를 유지하기 위해서 stateTime은 왠만하면 정하지 말자!

<br>

### 4.5.2 cacheTime

- 기본값: 5 _ 60 _ 1000ms (5분)
- 설명: 데이터가 캐시된 상태로 유지되는 시간을 밀리초 단위로 설정한다.
  - 쿼리 인스턴스가 unmount 되면 데이터는 inactive 상태로 변경되며, 캐시는 cacheTime만큼 유지된다.
  - cacheTime 지나기 전에 쿼리 인스턴스가 다시 mount 되면, 데이터를 fetch 하는 동안 캐시 데이터를 보여준다.
  - cacheTime은 staleTime과 관계없이, 무조건 inactive 된 시점을 기준으로 캐시 데이터 삭제를 결정한다.
  - 이 시간이 지나면 GC(가비지 콜렉터)에 의해 cache data는 삭제 처리된다.
- 특징: 데이터의 캐시를 얼마나 오래 유지할지를 결정한다.

```jsx
const { data, error, isLoading } = useQuery(
  "todos",
  fetchTodos,
  { cacheTime: 300000 } // 캐시된 데이터가 5분 동안 유지됨
);
```

⚠️ staleTime을 cacheTime보다 길게 설정했다고 가정하면, staleTime만큼의 캐싱을 기대했을 때 원하는 결과를 얻지 못할 것이다.

<br>

> useQuery 실행 시 cacheTime > 0 인 것과 queryFn 실행간의 관계?

- `cacheTime > 0` 인 것과 queryFn 실행은 상관 없다. staleTime 과 관계가 있다.
- `cacheTime > 0` 이면, 캐시 데이터가 존재하고, 이 경우 useQuery 를 실행할 때 stale data을 우선 받고 staleTime이 0이면 queryFn을 실행한 리턴값으로 리렌더링하면서 바꿔준다.
- cacheTime 이 0이 되면 캐시 데이터가 삭제되기 때문에, 이 경우 useQuery 로 data 호출 시 undefined 값을 우선 받고, queryFn을 실행한 리턴값으로 리렌더링하면서 바꿔준다.

<br>

### 4.5.3 enabled

- 기본값: true
- 설명: 쿼리가 활성화될 조건을 설정한다. false로 설정하면 쿼리는 자동으로 실행되지 않는다. (true인 경우에만 queryFn 실행)
- 특징: 특정 조건이 충족될 때만 쿼리가 실행되도록 할 수 있다. 즉, 쿼리를 수동으로 다시 요청한다.
- 예시: 보통 자동으로 쿼리 요청을 하지 않고 버튼 클릭이나 특정 이벤트를 통해 요청을 시도할 때 같이 사용한다.

```jsx
const { data, error, isLoading } = useQuery(
  "todos",
  fetchTodos,
  { enabled: !!userId } // userId가 있을 때만 쿼리가 실행됨
);
```

<br>

> 적용 예제(1): Disabling/Pausing Queries (이벤트 발생 시에만 수동 실행하고 싶을 때)

```jsx
const { data, refetch } = useQuery(["todos"], getTodos, {
  enabled: false,
});

return (
  <div>
    <button onClick={() => refetch()}>데이터 불러오기</button>
  </div>
);
```

<br>

> 적용 예제(2): Dependent Queries (useQuery 2개 이상이며 실행순서 설정 필요할 때)

{% raw %}

```jsx
// Dependent Query 예제 (순차적 query 실행)
// Get the user
const { data: user } = useQuery({
  queryKey: ["user", email],
  queryFn: getUserByEmail,
});

const userId = user?.id;

// Then get the user's projects
const {
  status,
  fetchStatus,
  data: projects,
} = useQuery({
  queryKey: ["projects", userId],
  queryFn: getProjectsByUser,
  // The query will not execute until the userId exists
  enabled: !!userId,
});
// 여기서 !!userId 는 Boolean(userId)와 같다.
```

{% endraw %}

<br>

### 4.5.4 select

- 기본값: undefined
- 설명: 쿼리 데이터를 반환하기 전에 변형하는 함수이다. 데이터의 형식을 변경하거나 필요한 정보만 추출할 때 사용한다.
- 특징: queryFn 에 의해 리턴된 값을 변형시킨 후에 useQuery 의 리턴 data로 넘겨준다. 단, 반환된 데이터 값에는 영향을 주지만 쿼리 캐시에 저장되는 내용에는 영향을 주지 않는다.

```jsx
const { data } = useQuery(
  "todos",
  fetchTodos,
  { select: (data) => data.filter((todo) => todo.completed) } // 완료된 todo만 반환
);
```

<br>

### 4.5.5 retry

- 기본값: 클라이언트 환경에서는 3, 서버 환경에서는 0
- 설명: 쿼리가 실패할 경우 재시도할 횟수를 결정하는 옵션이다. false로 설정하면 재시도하지 않으며, true인 경우에는 실패한 쿼리에 대해서 무한 재요청을 시도한다.
- 특징: `useQuery` 또는 `useInfiniteQuery`에 등록된 `queryFn`이 API 서버에 요청을 보내서 네트워크 오류 등으로 인해 실패하더라도 바로 에러를 띄우지 않고 자동으로 재시도할 수 있다.

```jsx
const { data, error } = useQuery(
  "todos",
  fetchTodos,
  { retry: 2 } // 실패 시 최대 2회 재시도
);
```

<br>

### 4.5.6 retryDelay

- 기본값: 1000 ms (1초)
- 설명: 재시도 간의 지연 시간을 밀리초 단위로 설정한다.
- 특징: 재시도 간의 대기 시간을 조정할 수 있다.

```jsx
const { data, error } = useQuery(
  "todos",
  fetchTodos,
  { retryDelay: 2000 } // 재시도 간의 지연 시간 2초
);
```

<br>

### 4.5.7 Polling

> 💡 Polling(폴링)이란?

- 실시간 웹을 위한 기법으로 "일정한 주기(특정한 시간)"를 가지고 서버와 응답을 주고받는 방식을 말한다.
- react-query에서는 "refetchInterval", "refetchIntervalInBackground"을 이용해서 구현할 수 있다.

```jsx
const { data } = useQuery("todos", fetchTodos, {
  refetchInterval: 60000, // 1분마다 자동으로 다시 패칭
  refetchIntervalInBackground: true, // 백그라운드 상태에서도 자동으로 다시 패칭
});
```

<br>

> refetchInterval

- 기본값: false
- 설명: 쿼리를 일정 간격으로 자동으로 다시 패칭한다. 밀리초 단위로 설정하며, false로 설정하면 자동 패칭이 비활성화된다.
- 특징: 실시간 데이터를 지속적으로 업데이트 해야 할 때 유용하다.

```jsx
const { data } = useQuery("todos", fetchTodos, {
  refetchInterval: 60000, // 1분마다 자동으로 다시 패칭
});
```

<br>

> refetchIntervalInBackground

- 기본값: false
- 설명: refetchInterval과 함께 사용하는 옵션이며, 브라우저가 백그라운드에 있을 때도 refetchInterval에 따라 쿼리를 자동으로 다시 패칭할지 여부를 설정한다.
- 특징: 브라우저가 백그라운드 상태에서도 일정 간격으로 데이터를 패칭하여 최신 상태를 유지하고자 할 때 유용하다.

<br>

### 4.5.8 refetchOnWindowFocus

- 기본값: true
- 설명: 데이터가 stale 상태일 경우, 브라우저 창이 포커스를 받을 때 쿼리를 자동으로 다시 패칭할지 여부를 설정한다.
- 특징
  - 브라우저 화면을 focus할 때마다 stale data를 자동으로 refetch 실행하여 최신 데이터를 유지하도록 한다.
  - 즉, 사용자가 브라우저 창이나 탭으로 다시 돌아왔을 때 자동으로 쿼리를 다시 실행할지 여부를 결정한다.

```jsx
const { data } = useQuery(
  "todos",
  fetchTodos,
  { refetchOnWindowFocus: false } // 브라우저 창이 포커스를 받아도 다시 패칭하지 않음
);
```

<br>

### 4.5.9 refetchOnMount

- 기본값: true
- 설명: 컴포넌트가 마운트될 때 쿼리를 자동으로 다시 패칭할지 여부를 설정한다.
- 특징: true로 설정하면, `useQuery` 또는 `useInfiniteQuery`가 있는 컴포넌트가 마운트 시 stale data를 자동으로 refetch를 실행하지만, false로 설정하면 최초 fetch 이후에는 refetch 하지 않는다.

```jsx
const { data } = useQuery(
  "todos",
  fetchTodos,
  { refetchOnMount: false } // 컴포넌트가 마운트될 때 다시 패칭하지 않음
);
```

<br>

### 4.5.10 refetchOnReconnect

- 기본값: true
- 설명: 네트워크가 재연결될 때 쿼리를 자동으로 다시 패칭할지 여부를 설정한다.
- 특징: 네트워크가 끊겼다가 재연결되었을 때 stale data를 자동으로 refetch 실행하여 데이터를 최신 상태로 유지한다.

```jsx
const { data } = useQuery(
  "todos",
  fetchTodos,
  { refetchOnReconnect: false } // 네트워크가 재연결될 때 다시 패칭하지 않음
);
```

<br>

### 4.5.11 notifyOnChangeProps

- 기본값: ['data']
- 설명: 쿼리 데이터가 변경될 때 알림을 받을 프로퍼티를 지정한다.
- 특징: 특정 프로퍼티의 변경 시에만 리렌더링을 트리거할 수 있다.

```jsx
const { data, error } = useQuery(
  "todos",
  fetchTodos,
  { notifyOnChangeProps: ["data"] } // 데이터가 변경될 때만 알림
);
```

<br>

### 4.5.12 onSuccess, onError, onSettled

- onSuccess: 쿼리가 성공적으로 완료되었을 때 호출되는 콜백 함수이다.
- onError: 쿼리 실행 중 오류가 발생했을 때 호출되는 콜백 함수이다.
- onSettled: 쿼리가 성공하든 실패하든 항상 호출되는 콜백 함수이다. 주로 캐시 무효화나 상태 업데이트에 사용된다.

```jsx
import { useQuery } from "react-query";
import axios from "axios";

const fetchTodos = async () => {
  const { data } = await axios.get("/api/todos");
  return data;
};

const TodosComponent = () => {
  const { data, error, isLoading } = useQuery(
    "todos", // 쿼리 키
    fetchTodos, // 데이터 패칭 함수
    {
      onSuccess: (data) => {
        // 패칭이 성공적으로 완료되었을 때 호출.
        console.log("Data fetched successfully:", data);
        // 예: 성공적으로 데이터를 패칭한 후 UI 업데이트
      },
      onError: (error) => {
        // 패칭 중 오류가 발생했을 때 호출
        console.error("Error fetching data:", error);
        // 예: 오류 메시지 표시
      },
      onSettled: (data, error) => {
        // 패칭이 성공하든 실패하든 호출
        if (error) {
          console.error("Error settled:", error);
        } else {
          console.log("Data settled:", data);
        }
        // 예: 쿼리 무효화, 데이터 캐시 무효화 등
      },
      // 선택적으로 추가 옵션을 설정할 수 있다.
      staleTime: 300000, // 데이터가 5분 동안 신선하다고 간주됨
      cacheTime: 600000, // 데이터 캐시를 10분 동안 유지
      retry: 2, // 실패 시 최대 2회 재시도
      refetchOnWindowFocus: false, // 브라우저 포커스 시 재패칭하지 않음
    }
  );

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>Todos</h1>
      <ul>
        {data.map((todo) => (
          <li key={todo.id}>{todo.title}</li>
        ))}
      </ul>
    </div>
  );
};

export default TodosComponent;
```

<br>

### 4.5.13 placeholderData

- 설명
  - 쿼리가 처음으로 로드될 때 사용할 초기 데이터를 설정한다. 이 데이터는 쿼리가 성공적으로 완료될 때까지 UI에 표시된다.
- 특징
  - 데이터가 로드되는 동안 사용자에게 표시할 기본값을 제공하여 더 나은 사용자 경험을 제공한다. 네트워크 요청이 완료되면 실제 데이터로 대체된다
  - 즉, placeholderData는 캐시에 유지되지 않으며, 서버 데이터와 관계없는 보여주기용 가짜 데이터다.

```jsx
const placeholderData = useMemo(() => generateFakeHeroes(), []);

const {
  data,
  // ...
} = useQuery({
  queryKey: ["super-heroes"],
  queryFn: getAllSuperHero,
  placeholderData: placeholderData,
});
```

<br><br>

# 5. useMutation 사용하여 데이터 변경하기

## 5.1 기본 기능

- `useMutation` 훅은 데이터 생성, 업데이트, 삭제(CUD)등의 변경 작업을 처리하는 데 사용된다.
- R(read)는 useQuery, CUD(Create, Update, Delete)는 useMutation을 사용한다.

<br>

## 5.2 사용 예시

- useMutation의 반환 값인 mutation 객체의 mutate 메서드를 이용해서 요청 함수를 호출할 수 있다.
- mutate는 onSuccess, onError 메서드를 통해 성공했을 시, 실패했을 시 response 데이터를 핸들링할 수 있다.

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query';
import axios from 'axios';

// 데이터 생성(POST) 예제 함수
const createTodo = async (newTodo) => {
  const { data } = await axios.post('http://localhost:5000/todos', newTodo);
  return data;
};

function AddTodo() {
  const queryClient = useQueryClient();

  // useMutation 훅을 사용하여 createTodo 함수를 비동기 요청으로 설정
  const { mutate } = useMutation({
		mutationFn: createTodo
    // 성공적으로 데이터가 생성된 후에 수행할 작업을 정의
    onSuccess: () => {
      // 'todos' 쿼리에 대한 캐시를 무효화하여, 새로운 데이터로 쿼리를 갱신
      queryClient.invalidateQueries(['todos']);
    },
  });

  const onSubmit = async () => {
    // mutation.mutate를 호출하여 비동기 요청을 실행
    // 여기서는 새 Todo 항목을 생성
		const nextTodo = {...}

    mutate(nextTodo);
  };

  // UI 컴포넌트와 이벤트 핸들러...
}

```

<br><br>

# 6. QueryClient

## 6.1 개념

- QueryClient는 React Query에서 쿼리와 관련된 상태, 캐시, 설정을 중앙에서 관리하는 클래스이다.
- 모든 쿼리와 뮤테이션은 QueryClient 인스턴스를 통해 수행된다.
- 애플리케이션 내에서 하나의 QueryClient 인스턴스를 생성하고, QueryClientProvider를 사용하여 React 컴포넌트 트리에 제공한다.

<br>

## 6.2 QueryClient의 속성

### 6.2.1 queryCache

- 설명: 쿼리 데이터를 저장하고 관리하는 캐시이다.
- 사용법: queryCache를 직접 접근하여 쿼리 상태를 확인하거나 조작할 수 있다.

<br>

### 6.2.2 mutationCache

- 설명: 뮤테이션 상태를 저장하고 관리하는 캐시이다.
- 사용법: mutationCache를 직접 접근하여 뮤테이션 상태를 확인하거나 조작할 수 있다.

```jsx
const queryClient = new QueryClient();
const mutationCache = queryClient.getMutationCache();
```

<br>

### 6.2.3 defaultOptions

- 설명: 쿼리와 뮤테이션에 대한 기본 옵션을 설정할 수 있다.
- 사용법: QueryClient를 생성할 때 defaultOptions를 설정하여 기본 옵션을 지정한다.

```jsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 60000,
    },
    mutations: {
      retry: 3,
    },
  },
});
```

<br>

## 6.3 QueryClient의 메서드

### 6.3.1 invalidateQueries

> 특정 쿼리('todos')의 캐시를 무효화한다. 이를 통해 데이터가 변경되었을 때 관련된 쿼리를 자동으로 갱신할 수 있다.

```jsx
queryClient.invalidateQueries("todos"); // 'todos' 쿼리를 무효화
```

<br>

### 6.3.2 refetchQueries

> 특정 쿼리를 즉시 재패칭한다.

```jsx
queryClient.resetQueries("todos"); // 'todos' 쿼리를 초기 상태로 재설정
```

<br>

### 6.3.3 resetQueries

> 특정 쿼리를 초기 상태로 재설정한다.

```jsx
queryClient.resetQueries("todos"); // 'todos' 쿼리를 초기 상태로 재설정
```

<br>

### 6.3.4 removeQueries

> 특정 쿼리를 캐시에서 완전히 제거한다.

```jsx
queryClient.removeQueries("todos"); // 'todos' 쿼리를 캐시에서 제거
```

<br>

### 6.3.5 cancelQueries

> 진행 중인 특정 쿼리의 요청을 취소한다.

```jsx
queryClient.cancelQueries("todos"); // 'todos' 쿼리의 진행 중인 요청을 취소
```

<br>

# 참조

- [[React Query 공식문서↗️]](https://tanstack.com/query/latest)
- [[TanStack Query(aka. react query) 에서 자주 사용되는 개념 정리↗️]](https://github.com/ssi02014/react-query-tutorial?tab=readme-ov-file#%EC%A3%BC%EC%9A%94-%EC%BB%A8%EC%85%89-%EB%B0%8F-%EA%B0%80%EC%9D%B4%EB%93%9C-%EB%AA%A9%EC%B0%A8)

<br>
