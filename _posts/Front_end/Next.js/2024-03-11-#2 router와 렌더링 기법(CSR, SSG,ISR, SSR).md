---
title: "[Next.js] #2 router와 렌더링 기법(CSR, SSG,ISR, SSR)"
categories: [Next.js]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. App router vs Pages route

## 1.1 Next.js의 버전의 주요 분기점

> Next.js를 채택할 때 주요한 의사결정은 "`App router`, `Pages router` 둘 중 어떤 router을 사용할 것인가?" 에 있다.

<br>

`Pages router`는 page 폴더를 기준으로 라우팅이 된다.

![](/assets/images/2024/2024-03-11-16-35-06.png)

<br>

`app router`는 app 폴더 밑에 폴더명을 기반으로 자동 라우팅이 된다.

![](/assets/images/2024/2024-03-11-16-32-58.png)

<br>

## 1.2 app router와 pages router의 사용

> app router와 pages router 어떤 것을 사용해야 할까?

- 공식 홈페이지에서는 `app router`를 사용하라고 제시하고 있다.
- `Pages Router`에서는 직접 캐싱을 구현해야 했지만, `App Router`를 사용시 캐싱처리를 해준다.

<br><br>

# 2. 기존 렌더링 방식

![렌더링 역사](</assets/images/2024/렌더링 역사.png>)

## 2.1 MPA(Multi page application)

- 원시적인 서버 사이드 렌더링 방식인 MPA로부터 프론트엔드 웹 개발은 시작되었다.
- 페이지 이동시 및 렌더링 시 깜빡거리는 현상이 있으므로 UX가 저하된다.
- 이러한 문제 때문에 React, Angular, Vue 등 SPA(Single Page Application)이 등장하였다.

```
/about → about.html
/profile → profile.html
```

<br>

## 2.2 SAR(Single page application)

> 브라우저에서 Javascript를 이용해 동적으로 페이지를 렌더링 하는 방식이다.

- "Client의 사이드에서 렌더링을 한다"라는 개념은 기존 프론트엔드 개발자들에게 획기적 방법으로 소개한다.
- 최초 서버로부터는 텅 빈, root라는 id를 가진 div만 다운로드 ⇒ javascript로 UI가 완성된다.
- 더 이상 새로고침이나 깜빡거림 없이 웹서비스 이용이 가능하여 UX가 크게 향상되지만,
  늦는 초기로딩속도라는 단점이 새롭게 대두되게 된다.
  - 이를 보완하기 위해 Code Spilitting(Lazy-Loading) 방법을 제시하게 된다.
  - 하나로 번들된 코드를 여러 코드로 나눠 당장 필요한 코드가 아니면 나중에 불러오는 방식이다.
  - 그럼에도 불구하고 여러 문제점이 발생 → Next.js로 문제점 해결!

<br><br>

# 3. Next.js의 주요 렌더링 기법

- build란 소스코드를 실행 가능한 상태로 만들어 놓은 과정을 말한다.
- `CSR`은 클라이언트 측에서 동적으로 콘텐츠를 생성하는 반면, `SSR`(서버 사이드 렌더링), `SSG`(정적 사이트 생성), `ISG`(점진적 정적 생성)는 서버에서 미리 렌더링된 HTML을 클라이언트에 제공한다.

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

> 코드

```js
import React from "react";
import ReactDOM from "react-dom";

function App() {
  return <h1>Hello, Client Side Rendering!</h1>;
}

// index.js
ReactDOM.render(<App />, document.getElementById("root"));
```

<br>

## 3.2 SSR(Server Side Rendering)📌

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

> 코드

```js
import React from "react";

function HomePage({ data }) {
  return <div>{data}</div>;
}

export async function getServerSideProps() {
  const res = await fetch("https://..."); // 외부 API 호출
  const data = await res.json();

  return { props: { data } };
}

export default HomePage;
```

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

> 코드(next.js 12 버전)

```js
import React from "react";

function HomePage({ data }) {
  return <div>{data}</div>;
}

export async function getStaticProps() {
  const res = await fetch("https://..."); // 외부 API 호출
  const data = await res.json();

  return { props: { data } };
}

export default HomePage;
```

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

> 코드

```js
import React from "react";

function HomePage({ data }) {
  return <div>{data}</div>;
}

export async function getStaticProps() {
  const res = await fetch("https://..."); // 외부 API 호출
  const data = await res.json();

  return {
    props: { data },
    revalidate: 60, // 1초 후에 페이지 재생성
  };
}

export default HomePage;
```

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

# 4. Hydration

> Hydration을 이해하기 위해 TTV, TTI를 알아야한다.

- TTV (Time To View): 사용자가 웹 페이지를 볼 수 있을 때까지의 시간
- TTI (Time To Interaction): 인터렉션(웹 페이지 간의 상호 작용: 클릭, 드래그 등)이 얼마나 빠르게 될 수 있는지

<br>

- 최초 SSR에 의해 화면이 그려짐 -> 아직 JS 파일을 다운받지 못해 인터렉션을 못하는 상태 (TTV이지만 TTI는 아닌 상태)
- 정적 페이지에 JS 소스 코드로 물(비유)을 마구 붙는 상태(Hydration) -> 인터렉션(클릭, 드래그 등)이 활성화(TTI 상태)

➡️ 즉, TTV와 TTI의 간격을 줄인 것이 Hydration이다.

<br>

- CSR
  - React에서 CSR로만 컴포넌트 렌더링을 할 때는 모든 React 소스파일을 바탕으로 한 자바스크립트 파일이 모두 다운로드 돼야만(즉, Hydration이 돼야만) 다운로드 받아야만 화면을 볼 수 있기 때문에 TTV가 오래 걸렸다.
- SSR
  - 서버에서는 사용자의 요청이 있을 때마다 페이지를 새로 그려서 사용자에게 제공한다.
  - 두 과정으로 나눠서 제공한다.
    1.  pre-rendering : 사용자와 상호작용하는 부분을 제외한 껍데기만을 먼저 브라우저에게 제공한다. (TTV가 엄청나게 빠름)
    2.  hydration : 이 과정이 일어나기 전까지는 껍데기만 있는 html 파일이기 때문에 사용자가 아무리 버튼을 click 해도 아무 동작이 일어나지 않는다. 인터렉션에 필요한 모든 파일을 다운로드 받는 과정 즉, hydration 과정이 끝나야 그제서야 인터렉션이 가능하다.
    3.  이 간극, TTI를 줄이는 것이 관건인 것이다.

➡️ SSG, ISR도 SSR과 마찬가지로 hydration 과정이 존재한다.

<br>
