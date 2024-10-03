---
title: "[Next.js] #2 Rendering 기법(CSR, SSG, ISR, SSR, Hydration)"
categories: [Next.js]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 기존 렌더링 방식

![렌더링 역사](</assets/images/2024/렌더링 역사.png>)

## 1.1 MPA(Multi page application)

- 원시적인 서버 사이드 렌더링 방식인 MPA로부터 프론트엔드 웹 개발은 시작되었다.
- 페이지 이동시 및 렌더링 시 깜빡거리는 현상이 있으므로 UX가 저하된다.
- 이러한 문제 때문에 React, Angular, Vue 등 SPA(Single Page Application)이 등장하였다.

```
/about → about.html
/profile → profile.html
```

<br>

## 1.2 SAR(Single page application)

> 브라우저에서 Javascript를 이용해 동적으로 페이지를 렌더링 하는 방식이다.

- "Client의 사이드에서 렌더링을 한다"라는 개념은 기존 프론트엔드 개발자들에게 획기적 방법으로 소개한다.
- 최초 서버로부터는 텅 빈, root라는 id를 가진 div만 다운로드 ⇒ javascript로 UI가 완성된다.
- 더 이상 새로고침이나 깜빡거림 없이 웹서비스 이용이 가능하여 UX가 크게 향상되지만,
  늦는 초기로딩속도라는 단점이 새롭게 대두되게 된다.
  - 이를 보완하기 위해 Code Spilitting(Lazy-Loading) 방법을 제시하게 된다.
  - 하나로 번들된 코드를 여러 코드로 나눠 당장 필요한 코드가 아니면 나중에 불러오는 방식이다.
  - 그럼에도 불구하고 여러 문제점이 발생 → Next.js로 문제점 해결!

<br><br>

# 2. 서버 렌더링과 fetch와 Rendering Type

## 2.1 서버 렌더링

> 서버에서 컴포넌트를 렌더링하여 HTML을 생성하고, 이를 클라이언트에 전달하는 방식을 말한다.

- Next의 기본 컴포넌트 생성 값이다.
- 초기 로드 시간이 빨라지고 SEO(검색 엔진 최적화)에 유리하다.
- 서버 컴포넌트는 데이터베이스나 API에서 데이터를 직접 가져와서 사용할 수 있어, 클라이언트 측에서 별도로 데이터를 패칭할 필요가 없다.
- 클라이언트 측에서는 필요한 최소한의 코드만 전달받으므로, 클라이언트 애플리케이션이 가벼워지고 로딩 속도가 빨라진다.
- `SSR`(서버 사이드 렌더링), `SSG`(정적 사이트 생성), `ISG`(점진적 정적 생성)이 해당된다.

<br>

> 나타난 이유

1. 어차피 클라이언트에서 렌더링하면 결과가 똑같은데 매번 다른 유저들이 각자 렌더링 할 필요가 있을까?
2. 서버에서 미리 렌더링해서 결과만 보내주면 되지 않을까?

<br>

> 라면으로 비유를 하자면,

→ 라면 끓일 때 스프가 필요하다!

고객한테 스프 만드는 데에 필요한 재료를 함께 주기(클라이언트 렌더링) vs 공장에서 미리 만든 스프 주기(서버 렌더링)

<br>

## 2.2 fetch와 Rendering Type

> 컴포넌트의 변화는 <span style="color:CornflowerBlue">fetch한 데이터를 기준으로 변경</span>이 되기 때문에 fetch 데이터의 변경은 컴포넌트 렌더링 방식을 규정한다고 볼 수 있다.

- 즉, fetch를 통해 여러가지 렌더링 패턴에 대해 이해할 수 있는 것이다.
- `SSR`(서버 사이드 렌더링), `SSG`(정적 사이트 생성), `ISG`(점진적 정적 생성)은 모두 <span style="color:CornflowerBlue">서버에서 렌더링을 처리</span>하는 방식이지만, 각 방법은 <span style="color:CornflowerBlue">다른 렌더링 시점</span>을 가지고 있다.

| Rendering Type                      | 데이터 변화 유형                                                                                 | 동작 방식                                                                                                                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CSR (Client Side Rendering)         | 실시간으로 계속 바뀌는 데이터, 컴포넌트 요청이 있을 때 마다 데이터를 갱신해서 최신 데이터만 제공 | CSR은 빌드타임에 컴포넌트를 초기 생성하지 않는다. 클라이언트 측에서 Javascript로 이루어진 리액트 파일을 다운로드 받고, 요청이 있을 때마다 데이터를 갱신하여 최신 데이터를 제공한다.  |
| SSG (Static Site Generation)        | 영원히 변하지 않는 데이터                                                                        | SSG는 빌드타임(build-time)에만 컴포넌트를 생성하고 이후에는 페이지가 변하지 않는 것으로 가정하여 정적 컴포넌트를 제공한다. Next.js는 아무것도 하지 않으면 기본적으로 SSG로 동작한다. |
| ISR (Incremental Site Regeneration) | 가끔씩만 변하는 데이터, 일정 주기마다 가끔씩만 컴포넌트를 갱신                                   | ISR은 빌드타임(build-time)에 컴포넌트를 초기 생성하고, 이후에는 일정 주기마다 변화를 적용하여 컴포넌트를 갱신한다.                                                                   |
| SSR (Server Side Rendering)         | 실시간으로 계속 바뀌는 데이터, 컴포넌트 요청이 있을 때 마다 데이터를 갱신해서 최신 데이터만 제공 | SSR은 빌드타임(build-time)에 컴포넌트를 초기 생성하고, 사용자의 요청이 있을 때마다 서버에서 최신 데이터를 가져와 적용하여 동적으로 컴포넌트를 제공한다.                              |

<br>

## 2.3 서버 렌더링 개발자 도구로 확인하기

> 서버 렌더링일 경우 `개발자 도구(F12) > NetWork` 탭을 통해 확인이 가능하다!

이처럼 서버 렌더링은 서버에서 컴포넌트를 렌더링하여 HTML을 생성하기 때문에 SEO(검색 엔진 최적화)에 유리한 것이다.

![](/assets/images/2024/2024-09-23-19-42-30.png)

<br>

<br><br>

# 3. Next.js의 주요 렌더링 기법

## 3.1 CSR(Client Side Rendering)

> 특징

- 브라우저에서 JavaScript를 이용해 동적으로 페이지를 렌더링하는 방식
- 렌더링의 주체 : 클라이언트
- 순수 리액트 사용했을 때

> 장점

- (최초 한번 로드가 끝나면) 사용자와의 상호작용이 빠르고 부드럽다.
- 서버에게 추가적인 요청을 보낼 필요가 없기 때문에(이미 다 다운로드를 받았기 때문), 사용자 경험(UX)이 좋다.
- 서버 부하가 적다.(최초 1회만 요청하면 되기 때문)

> 단점

- 첫 페이지 로딩 시간(Time To View)이 길 수 있다.
- JavaScript가 로딩되고 실행될 때까지 페이지가 비어있어 검색 엔진 최적화(SEO)에 불리하다. (div 하나만 보이기 때문)

<br>

## 3.2 SSR(Server Side Rendering)

> 특징

- 사용자의 요청이 있을 때마다 서버에서 페이지의 HTML을 생성한다.
- 즉, 사용자가 페이지에 접근할 때마다 서버는 최신의 데이터를 반영하여 HTML을 생성하고 전송한다.

> 장점

- SEO(검색 엔진 최적화)에 유리하다.
- 실시간 데이터를 사용한다.
- 빠른 로딩 속도(TTV)와 높은 보안성을 제공한다.

> 단점

- 각 요청마다 서버에서 페이지를 생성해야 하기 때문에 **서버 부하가 증가**할 수 있다.
- 사이트의 콘텐츠가 변경되면 전체 사이트를 다시 빌드해야 하는데, 이 과정이 시간이 오래 걸릴 수 있다.

> 예시

- 뉴스 사이트 -> 속도 중요 (실시간 업데이트 필요)

<br>

## 3.3 SSG(Static Site Generation)

> 특징

- **빌드 타임**에 모든 페이지를 미리 HTML로 생성한다.
- 클라이언트가 홈페이지 요청을 하면, 서버에서는 이미 만들어져있는 사이트를 바로 제공하기 때문에 빠른 로딩 시간을 보장한다.

> 장점

- 빠른 로딩 속도와 낮은 서버 부하, 탁월한 캐싱 성능으로 인해 고정된 데이터를 다루는 사이트에 적합하다.
- SEO에 유리하다.
- CDN(Content Delivery Network) 캐싱이 가능하다.

> 단점

- 실시간 데이터 반영이 어렵기 때문에 정적인 데이터에만 사용할 수 있다.
- 사이트의 모든 페이지를 빌드 타임에 생성해야 하므로 빌드 시간이 길어질 수 있다.

> 예시

- 회사 웹사이트 같은 경우 실시간 업데이트 필요x -> 제일 빠름

<br>

## 3.4 ISR(Incremental Static Regeneration)

> 특징

- SSG의 확장 개념으로 빌드 시 일부 페이지만 미리 생성하고, 나머지 페이지는 사용자의 요청에 따라 점진적으로 생성하여 캐싱한다.
- SSG의 한 번 빌드하면 결과물은 변하지 않지만, ISR은 설정한 주기만큼 페이지를 계속 생성해 준다.
- 즉, 정적 페이지를 먼저 보여주고, 필요에 따라 서버에서 페이지를 재생성하는 방식이다.

> 장점

- 정적 페이지를 먼저 제공하므로 사용자 경험이 좋으며, 콘텐츠가 변경되었을 때 서버에서 페이지를 재생성하므로 최신 상태를 (그나마) 유지할 수 있다.
- CDN 캐싱이 가능하다.

> 단점

- 동적인 콘텐츠를 다루기에 한계가 있을 수 있다.
- 이지 업데이트 주기를 잘 관리해야 한다.

> 예시

- 블로그를 개발했다고 쳤을 때 설정한 시간 후 새로운 포스팅 업데이트

<br>

> 주요 렌더링 기법을 정리하면 다음과 같다.

|                              | CSR  | SSR  | SSG  | ISR          |
| ---------------------------- | ---- | ---- | ---- | ------------ |
| 빌드 시간                    | 짧다 | 짧다 | 길다 | 길다         |
| SEO                          | 나쁨 | 좋음 | 좋음 | 좋음         |
| 페이지 요청에 따른 응답 시간 | 보통 | 길다 | 짧다 | 짧다         |
| 최신 정보인가?               | 맞음 | 맞음 | 아님 | 아닐 수 있음 |

<br>

> http://www.sonnetfilm.com/about 페이지를 분석해보자.

- 거의 변화가 없는 ABOUT page의 경우 `SSG`로,
- 일주일에 한 번 업로드 될수있는 FILMS page의 경우 `ISR`로,
- 실시간으로 올라오는 예약 정보들을 열람해야 하는 RESERVATION page의 경우 `SSR`로 처리하는 것이 좋다.

SSG -> ISR -> SST 순으로 빈도가 많다.

<br><br>

# 4. 렌더링 패턴 4가지 구현하기(with fetch)⭐

> 원활한 테스팅을 위해서 `dev`모드가 아닌 `production mode`로 진행해야 한다. (dev모드에선 SSR처럼 동작하기 때문)

```shell
yarn build
```

```shell
yarn start
```

<br>

## 4.1 CSR

> fetch시, 요청이 있을 때 마다 지속해서 갱신해주면 CSR이다.

- `"use client";` 가 존재하면 CSR로 간주된다.
- 클라이언트 측에서 `useEffect`, `useState`를 통해 데이터를 가져온다.
- 즉, 그냥 순수 리액트 사용했을 때 CSR이다.

```tsx
"use client"; // 옵션 부여

import TodoList from "../components/todo/TodoList";
import TodoForm from "../components/todo/TodoForm";
import { Todo } from "@/app/types/todo.type";
import { useEffect, useState } from "react";

const TodoPage = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const loadTodos = async () => {
      try {
        const response = await fetch("http://localhost:4000/todos");
        const todos = await response.json();
        setTodos(todos);
      } catch (error) {
        console.error(error);
      } finally {
        setLoading(false);
      }
    };
    loadTodos();
  }, []);

  if (loading) {
    return <p>로딩중...</p>;
  }

  // 필터링
  const inProgressTodo = todos.filter((todo) => todo.isDone === false);
  const doneTodo = todos.filter((todo) => todo.isDone === true);

  return (
    <>
      <TodoForm />
      <TodoList todoTitle="In Progress" todos={inProgressTodo} />
      <TodoList todoTitle="Done" todos={doneTodo} />
    </>
  );
};

export default TodoPage;
```

<br>

## 4.2 SSR

> fetch시, 요청이 있을 때 마다 지속해서 갱신해주면 SSR이다. (캐시된 데이터 취급x)

- 매번 서버로부터 최신 데이터를 가져온다. (`cache: "no-cache"` 옵션을 사용).
- hydration이 완료되기 전까지의 시간. 즉, TTI(Time To Interactive)가 관건이다.
- 클라이언트 사이드 렌더링 시에는 `useEffect`와 `useState`를 사용했지만, 서버 사이드 렌더링 시에는 컴포넌트 자체를 `async` 함수로 작성하여 서버에서 데이터를 가져온다.

```tsx
import TodoList from "../components/todo/TodoList";
import TodoForm from "../components/todo/TodoForm";
import { Todo } from "@/app/types/todo.type";

// 컴포넌트가 async 함수로 작성되어 서버에서 데이터를 가져온다. ->  next에선 서버 컴포넌트이므로 가능하다!
const TodoPage = async () => {
  const response = await fetch("http://localhost:4000/todos", {
    cache: "no-cache", // 옵션 부여(캐시를 하지 않음!)
  });
  const todos: Todo[] = await response.json();

  const inProgressTodo = todos.filter((todo) => !todo.isDone);
  const doneTodo = todos.filter((todo) => todo.isDone);

  return (
    <>
      <TodoForm />
      <TodoList todoTitle="In Progress" todos={inProgressTodo} />
      <TodoList todoTitle="Done" todos={doneTodo} />
    </>
  );
};

export default TodoPage;
```

<br>

## 4.3 SSG

> fetch시, 아무리 새로고침을 하여도 동일한 페이지만 출력되면 SSG이다.(브라우저 갱신x)

-한 번 빌드 시, 모든 데이터가 정적으로 생성되어 페이지가 동일하게 출력된다.

```tsx
import TodoList from "../components/todo/TodoList";
import TodoForm from "../components/todo/TodoForm";
import { Todo } from "@/app/types/todo.type";

const TodoPage = async () => {
  const response = await fetch("http://localhost:4000/todos");
  const todos: Todo[] = await response.json();

  const inProgressTodo = todos.filter((todo) => !todo.isDone);
  const doneTodo = todos.filter((todo) => todo.isDone);

  return (
    <>
      <TodoForm />
      <TodoList todoTitle="In Progress" todos={inProgressTodo} />
      <TodoList todoTitle="Done" todos={doneTodo} />
    </>
  );
};

export default TodoPage;
```

<br>

> 아무 옵션을 주지 않으면 fetch에 `force-cache`라는 옵션을 부여한것과 똑같다. (디폴트 값)

`force-cache` 옵션은 브라우저가 요청 시 캐시를 사용하도록 강제한다. (최신 데이터 못가져옴)

```tsx
const response = await fetch("http://localhost:4000/todos", {
  cache: "force-cache", // default
});
```

<br>

## 4.4 ISR

> fetch시, 주어진 시간에 한 번씩 갱신해주면 ISR이다.

방법 1: fetch에 `revalidate` 옵션 추가

```tsx
import TodoList from "../components/todo/TodoList";
import TodoForm from "../components/todo/TodoForm";
import { Todo } from "@/app/types/todo.type";

const TodoPage = async () => {
  const response = await fetch("http://localhost:4000/todos", {
    next: {
      revalidate: 5, // 5초마다 새로운 데이터 가져옴
    },
  });
  const todos: Todo[] = await response.json();

  const inProgressTodo = todos.filter((todo) => !todo.isDone);
  const doneTodo = todos.filter((todo) => todo.isDone);

  return (
    <>
      <TodoForm />
      <TodoList todoTitle="In Progress" todos={inProgressTodo} />
      <TodoList todoTitle="Done" todos={doneTodo} />
    </>
  );
};

export default TodoPage;
```

<br>

방법 2: fetch에 옵션을 주지 않고 페이지 컴포넌트에 직접 revalidate 설정

page.tsx 컴포넌트에 `revalidate` 추가하고 next 옵션은 지워주자

```tsx
import TodoList from "../components/todo/TodoList";
import TodoForm from "../components/todo/TodoForm";
import { Todo } from "@/app/types/todo.type";

const TodoPage = async () => {
  const response = await fetch("http://localhost:4000/todos"); // 옵션x
  const todos: Todo[] = await response.json();

  const inProgressTodo = todos.filter((todo) => !todo.isDone);
  const doneTodo = todos.filter((todo) => todo.isDone);

  return (
    <>
      <TodoForm />
      <TodoList todoTitle="In Progress" todos={inProgressTodo} />
      <TodoList todoTitle="Done" todos={doneTodo} />
    </>
  );
};

export default TodoPage;
```

<br>

그리고 TodoPage 를 import 하는 page에다 `revalidate`을 부여하자!

```tsx
import TopPage from "@/components/rendering/TopPage";

export const revalidate = 5;

const TopPage = () => {
  return (
    <>
      <TodoPage />
    </>
  );
};

export default TopPage;
```

- 방법1과 방법2는 똑같이 동작하지만, 방법2는 컴포넌트 레벨에선 적용이 불가능하고 next에서 정의한 `page.tsx` 또는 `layout.tsx`에서만 동작한다.
- 즉, 페이지에 포함되어있는 모든 데이터가 10초마다 데이터를 갱신하게 되기 때문에 페이지 레벨로 revalidate를 적용할 떄 유용한 기능이다.

<br><br>

# 5. Hydration

## 5.1 Hydration 개요

> 하이드레이션(Hydration)은 서버에서 렌더링된 HTML을 클라이언트에서 React 상태와 연결해 동작 가능하게 만드는 과정을 말한다.

아래 코드를 보자(Hydration 구현 방법은 [[여기↗️]](https://mynamesieun.github.io/next.js/React-Query%EC%97%90%EC%84%9C-SSR-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0(with-Streaming,-Hydration)/) 포스팅을 확인하자)

```tsx
// 서버 컴포넌트로 정의
const HomePage = async () => {
  const queryClient = new QueryClient();

  // 서버에서 데이터 프리패칭
  await queryClient.prefetchQuery({
    queryKey: [QUERY_KEYS.TODOS],
    queryFn: fetchTodos,
  });

  return (
    <main>
      {/* 미리 프리패칭된 데이터를 클라이언트로 전달 */}
      <HydrationBoundary state={dehydrate(queryClient)}>
        <TodoList />
      </HydrationBoundary>
    </main>
  );
};

export default HomePage;
```

- 💡 위 코드는 서버 컴포넌트이므로 서버에서 디하이드레이션된 상태(물을 채우지 않은 상태)로 데이터를 보내고, 클라이언트에서 그 데이터를 "하이드레이트"(컴포넌트에 물을 채움)하면서 상태를 활성화하는 것이다.
- 즉, 서버에서는 데이터를 준비만 하고, 클라이언트가 그것을 받아서 실제로 렌더링을 완성하는 작업을 하게 된다.

<br>

## 5.2 TTV와 TTI 그리고 Hydration의 관계

> Hydration을 자세히 이해하기 위해 TTV, TTI를 알아야한다.

- TTV (Time To View):
  - 사용자가 최초로 웹 페이지를 보게 되는 데에까지 걸리는 시간
- TTI (Time To Interaction):
  - 유저가 최초로 페이지와 상호작용 할 때 까지 걸리는 시간
  - 즉, 인터렉션(웹 페이지 간의 상호 작용: 클릭, 드래그 등)이 얼마나 빠르게 될 수 있는지

<br>

> TTV이지만 TTI는 아닌 상태

최초 SSR에 의해 화면이 그려짐 -> 아직 JS 파일을 다운받지 못해 인터렉션을 못하는 상태

> TTI 상태

정적 페이지에 JS 소스 코드로 물(비유)을 마구 붙는 상태(Hydration) -> 인터렉션(클릭, 드래그 등)이 활성화

<br>

➡️ 즉, TTV와 TTI의 간격을 줄인 것이 <span style="color:indianred">Hydration</span>이다.<br>

<br>

## 5.3 CSR과 SSR의 Hydration

> CSR

- React에서 CSR로만 컴포넌트 렌더링을 할 때는 모든 React 소스파일을 바탕으로 한 자바스크립트 파일이 모두 다운로드 돼야만(즉, Hydration이 돼야만) 다운로드 받아야만 화면을 볼 수 있기 때문에 TTV가 오래 걸렸다.

> SSR

SSR 환경에서는 사용자의 요청이 있을 때마다 서버에서 페이지를 새로 렌더링하여 사용자에게 제공한다. 이때 데이터 패칭 과정은 두 단계로 나뉜다.

- Pre-rendering
  - 서버에서 미리 HTML을 생성하여 클라이언트에 전달하는 방식(사용자와 상호작용하는 부분을 제외한 껍데기만을 먼저 브라우저에게 제공)
  - 그 HTML이 생성되기 위해 데이터가 필요할 때 prefetch를 사용한다.
  - TTV가 엄청나게 빠르다.
- hydration
  - 클라이언트에서 서버로부터 받은 HTML을 받아 React 컴포넌트와 상태를 연결(하이드레이션)하여, 페이지가 인터랙티브하게 작동할 수 있도록 만드는 과정
  - 이때 서버에서 미리 패칭된 데이터가 클라이언트에서 다시 "활성화" 된다.
  - 이 과정이 일어나기 전까지는 껍데기만 있는 html 파일이기 때문에 사용자가 아무리 버튼을 click 해도 아무 동작이 일어나지 않는다.

💡 prefetch는 서버에서 데이터를 미리 가져오는 작업이고, hydration은 그 데이터를 클라이언트에서 활성화하는 작업이다.<br>
💡 즉, hydration 과정이 끝나야 그제서야 인터렉션이 가능한데, 이 간극, TTI를 줄이는 것이 관건이다!

SSG, ISR도 SSR과 마찬가지로 hydration 과정이 존재한다.

<br>
