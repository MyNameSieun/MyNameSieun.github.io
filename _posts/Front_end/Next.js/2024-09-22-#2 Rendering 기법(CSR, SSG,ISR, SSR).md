---
title: "[Next.js] #2 Rendering 기법(CSR, SSG,ISR, SSR)"
categories: [Next.js]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Next.js v12와 v13의 주요 차이점

> Next.js에서 가장 중요한 개념이 바로 렌더링 방식이다.⭐(CSR, SSR, ISR, SSG)

- Next.js 버전 12까지는 렌더링 방식을 "페이지 단위"로 규정하였다.
  - about 페이지는 SSG로 동작
  - todolist 페이지는 SSR로 동작
  - sample 페이지는 CSR로 동작

<br>

하지만 Next.js 13 버전에서는 React 18 버전에서 제시한

- 서버컴포넌트(서버상에서만 동작 → node.js 환경)
- 클라이언트컴포넌트(브라우저에서만 동작 → 브라우저 환경)

을 토대로 렌더링 방식이 기존 "페이지 단위"가 아닌 "컴포넌트 단위"로 변경되었다. 컴포넌트 하나 하나가 각각의 렌더링 방식을 가질 수 있게 되었다!!

<br>

> 정리

- Next.js 버전 12는 `페이지 단위`가 렌더링 기준
- Next.js 버전 13은 `컴포넌트 단위`가 렌더링 기준⭐

![](/assets/images/2024/2024-03-12-00-02-01.png)

![](/assets/images/2024/2024-03-12-00-02-11.png)

위 그림과 같이 서버컴포넌트와 클라이언트컴포넌트가 공존이 가능하다.

<br>

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

# 3. 서버 렌더링과 fetch와 Rendering Type

## 3.1 서버 렌더링

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

## 3.2 fetch와 Rendering Type

> 컴포넌트의 변화는 fetch한 데이터를 기준으로 변경이 되기 때문에 fetch 데이터의 변경은 컴포넌트 렌더링 방식을 규정한다고 볼 수 있다.

- 즉, fetch를 통해 여러가지 렌더링 패턴에 대해 이해할 수 있는 것이다.
- `SSR`(서버 사이드 렌더링), `SSG`(정적 사이트 생성), `ISG`(점진적 정적 생성)은 모두 서버에서 렌더링을 처리하는 방식이지만, 각 방법은 다른 렌더링 시점을 가지고 있다.

| Rendering Type                      | 데이터 변화 유형                                                                                 | 동작 방식                                                                                                                                                                            |
| ----------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| CSR (Client Side Rendering)         | 실시간으로 계속 바뀌는 데이터, 컴포넌트 요청이 있을 때 마다 데이터를 갱신해서 최신 데이터만 제공 | CSR은 빌드타임에 컴포넌트를 초기 생성하지 않는다. 클라이언트 측에서 Javascript로 이루어진 리액트 파일을 다운로드 받고, 요청이 있을 때마다 데이터를 갱신하여 최신 데이터를 제공한다.  |
| SSG (Static Site Generation)        | 영원히 변하지 않는 데이터                                                                        | SSG는 빌드타임(build-time)에만 컴포넌트를 생성하고 이후에는 페이지가 변하지 않는 것으로 가정하여 정적 컴포넌트를 제공한다. Next.js는 아무것도 하지 않으면 기본적으로 SSG로 동작한다. |
| ISR (Incremental Site Regeneration) | 가끔씩만 변하는 데이터, 일정 주기마다 가끔씩만 컴포넌트를 갱신                                   | ISR은 빌드타임(build-time)에 컴포넌트를 초기 생성하고, 이후에는 일정 주기마다 변화를 적용하여 컴포넌트를 갱신한다.                                                                   |
| SSR (Server Side Rendering)         | 실시간으로 계속 바뀌는 데이터, 컴포넌트 요청이 있을 때 마다 데이터를 갱신해서 최신 데이터만 제공 | SSR은 빌드타임(build-time)에 컴포넌트를 초기 생성하고, 사용자의 요청이 있을 때마다 서버에서 최신 데이터를 가져와 적용하여 동적으로 컴포넌트를 제공한다.                              |

<br>

# 4. Next.js의 주요 렌더링 기법

## 4.1 CSR(Client Side Rendering)

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

## 4.2 SSR(Server Side Rendering)

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

## 4.3 SSG(Static Site Generation)

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

## 4.4 ISR(Incremental Static Regeneration)

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

# 5. 렌더링 패턴 4가지 구현하기(with fetch)⭐

## 5.1 실습 전 세팅

> 먼저 실습을 위한 세팅을 하자

- `src > app > rendering > page.tsx` 파일 만들기( page 컴포넌트 변경하면서 테스트를 진행하면 된다.)

  ```tsx
  import React from "react";
  import SSG from "@/components/rendering/SSG";

  const RenderingPage = () => {
    return (
      <div>
        <h1>4가지 렌더링 방식 테스트하자!!</h1>
        <SSG />
      </div>
    );
  };

  export default RenderingPage;
  ```

  <br>

- 각 렌더링 파일 만들기

  - src >components > rendering > SSG.tsx
  - src > components > rendering > ISR.tsx
  - src > components > rendering > SSR.tsx
  - src > components > rendering > CSR.tsx

  <br>

- src > types > index.ts 폴더를 만들고 아래 코드 붙여넣기

  ```tsx
  export type RandomUser = {
    gender: string;
    name: {
      title: string;
      first: string;
      last: string;
    };
    location: {
      street: {
        number: number;
        name: string;
      };
      city: string;
      state: string;
      country: string;
      postcode: string;
      coordinates: {
        latitude: string;
        longitude: string;
      };
      timezone: {
        offset: string;
        description: string;
      };
    };
    email: string;
    login: {
      uuid: string;
      username: string;
      password: string;
      salt: string;
      md5: string;
      sha1: string;
      sha256: string;
    };
    dob: {
      date: string;
      age: number;
    };
    registered: {
      date: string;
      age: number;
    };
    phone: string;
    cell: string;
    id: {
      name: string;
      value: string;
    };
    picture: {
      large: string;
      medium: string;
      thumbnail: string;
    };
    nat: string;
  };
  ```

app 폴더의 하위 컴포넌트들만 라우팅에 관여! 따라서 components 폴더는 라우팅에 관여하지 않는다.

<br>

> 원활한 테스팅을 위해서 dev모드가 아닌 production mode로 진행해야 한다. (dev모드에선 SSR처럼 동작)

```
yarn build
```

```
yarn start
```

<br>

## 5.2 SSG

> fetch시, 아무리 새로고침을 하여도 동일한 페이지만 출력되면 SSG(브라우저 갱신x)

SSG는 리액트 컴포넌트인데 async로 동작한다!(신기) next에선 서버 컴포넌트이므로 가능하다!

<br>

```tsx
import Image from "next/image";
import React from "react";

import type { RandomUser } from "@/types";

const SSG = async () => {
  const response = await fetch(`https://randomuser.me/api`);
  const { results } = await response.json();
  const user: RandomUser = results[0];

  return (
    <div className="mt-8">
      <div className="border p-4 my-4">
        <div className="flex gap-8">
          {/* 유저 기본정보 */}
          <div>
            <Image
              src={user.picture.large}
              alt={user.name.first}
              width={200}
              height={200}
            />
            <h2 className="text-xl font-bold">
              {user.name.title}. {user.name.first} {user.name.last}
            </h2>
            <p className="text-gray-600">{user.email}</p>

            <div className="mt-4">
              <p>
                <span className="font-bold">전화번호 : </span>
                {user.phone}
              </p>
              <p>
                <span className="font-bold">휴대전화번호 : </span>
                {user.cell}
              </p>
              <p>
                <span className="font-bold">사는 곳 : </span>
                {user.location.city}, {user.location.country}
              </p>
              <p>
                <span className="font-bold">등록일자 : </span>
                {new Date(user.registered.date).toLocaleDateString()}
              </p>

              <p>
                <span className="font-bold">생년월일 : </span>
                {new Date(user.dob.date).toLocaleDateString()}
              </p>
            </div>
          </div>

          {/* 지도영역 */}
          <iframe
            src={`https://maps.google.com/maps?q=${user.location.coordinates.longitude},${user.location.coordinates.latitude}&z=15&output=embed`}
            height="450"
            width="600"
          ></iframe>
        </div>
      </div>
    </div>
  );
};

export default SSG;
```

- 아무 옵션을 주지 않은 것은 fetch에 force-cache라는 옵션을 부여한것과 똑같다.(디폴트 값 )
- force-cache 옵션은 브라우저가 요청 시 캐시를 사용하도록 강제한다.(최신 데이터 못가져옴)

```tsx
const response = await fetch(`https://randomuser.me/api`, {
  cache: "force-cache",
});
```

<br>

## 5.3 ISR

> fetch시, 주어진 시간에 한 번씩 갱신해주면 ISR

(1) 첫 번째 방법 : fetch에 옵션 주기

```tsx
import Image from "next/image";
import React from "react";

import type { RandomUser } from "@/types";

const ISR = async () => {
  const response = await fetch(`https://randomuser.me/api`, {
    next: {
      revalidate: 5, // 5초마다 새로운 데이터 가져옴
    },
  });
  const { results } = await response.json();
  const user: RandomUser = results[0];

  return (
    <div className="mt-8">
      <div className="border p-4 my-4">
        <div className="flex gap-8">
          {/* 유저 기본정보 */}
          <div>
            <Image
              src={user.picture.large}
              alt={user.name.first}
              width={200}
              height={200}
            />
            <h2 className="text-xl font-bold">
              {user.name.title}. {user.name.first} {user.name.last}
            </h2>
            <p className="text-gray-600">{user.email}</p>

            <div className="mt-4">
              <p>
                <span className="font-bold">전화번호 : </span>
                {user.phone}
              </p>
              <p>
                <span className="font-bold">휴대전화번호 : </span>
                {user.cell}
              </p>
              <p>
                <span className="font-bold">사는 곳 : </span>
                {user.location.city}, {user.location.country}
              </p>
              <p>
                <span className="font-bold">등록일자 : </span>
                {new Date(user.registered.date).toLocaleDateString()}
              </p>

              <p>
                <span className="font-bold">생년월일 : </span>
                {new Date(user.dob.date).toLocaleDateString()}
              </p>
            </div>
          </div>

          {/* 지도영역 */}
          <iframe
            src={`https://maps.google.com/maps?q=${user.location.coordinates.longitude},${user.location.coordinates.latitude}&z=15&output=embed`}
            height="450"
            width="600"
          ></iframe>
        </div>
      </div>
    </div>
  );
};

export default ISR;
```

<br>

(2) 두 번째 방법 : page.tsx 컴포넌트에 revalidate 추가하기

ISR.tsx에 있는 next 옵션은 지워주자

```tsx
import Image from "next/image";
import React from "react";

import type { RandomUser } from "@/types";

const ISR = async () => {
  const response = await fetch(`https://randomuser.me/api`); // 옵션x
  const { results } = await response.json();
  const user: RandomUser = results[0];

  return (
    <div className="mt-8">
      <div className="border p-4 my-4">
        <div className="flex gap-8">
          {/* 유저 기본정보 */}
          <div>
            <Image
              src={user.picture.large}
              alt={user.name.first}
              width={200}
              height={200}
            />
            <h2 className="text-xl font-bold">
              {user.name.title}. {user.name.first} {user.name.last}
            </h2>
            <p className="text-gray-600">{user.email}</p>

            <div className="mt-4">
              <p>
                <span className="font-bold">전화번호 : </span>
                {user.phone}
              </p>
              <p>
                <span className="font-bold">휴대전화번호 : </span>
                {user.cell}
              </p>
              <p>
                <span className="font-bold">사는 곳 : </span>
                {user.location.city}, {user.location.country}
              </p>
              <p>
                <span className="font-bold">등록일자 : </span>
                {new Date(user.registered.date).toLocaleDateString()}
              </p>

              <p>
                <span className="font-bold">생년월일 : </span>
                {new Date(user.dob.date).toLocaleDateString()}
              </p>
            </div>
          </div>

          {/* 지도영역 */}
          <iframe
            src={`https://maps.google.com/maps?q=${user.location.coordinates.longitude},${user.location.coordinates.latitude}&z=15&output=embed`}
            height="450"
            width="600"
          ></iframe>
        </div>
      </div>
    </div>
  );
};

export default ISR;
```

<br>

그리고 ISR을 import 하는 page에다 revalidate을 부여하자!

```tsx
// src > app > rendering > page.tsx

import ISR from "@/components/rendering/ISR";
import React from "react";

export const revalidate = 5;

const RenderingTestPage = () => {
  return (
    <div>
      <h1>4가지 렌더링 방식을 테스트합니다.</h1>
      <ISR />
    </div>
  );
};

export default RenderingTestPage;
```

- 방법1과 방법2는 똑같이 동작하지만, 방법2는 컴포넌트 레벨에선 적용이 불가능하고 next에서 정의한 page.tsx 또는 layout.tsx에서만 동작한다.
- 즉, 페이지에 포함되어있는 모든 데이터가 10초마다 데이터를 갱신하게 되기 때문에 페이지 레벨로 revalidate를 적용할 떄 유용한 기능이다.

<br>

## 5.4 SSR

> fetch시, 요청이 있을 때 마다 지속해서 갱신해주면 SSR(캐시된 데이터 취급x) -> 서버 사이드 렌더링

hydration이 완료되기 전까지의 시간. 즉, TTI(Time To Interactive)가 관건이다.

<br>

fetch에 옵션 주기

```tsx
// src > components > rendering > SSR.tsx
import Image from "next/image";
import React from "react";

import type { RandomUser } from "@/types";

const SSR = async () => {
  const response = await fetch(`https://randomuser.me/api`, {
    cache: "no-cache", // 옵션 부여(캐시를 하지 않음!)
  });
  const { results } = await response.json();
  const user: RandomUser = results[0];

  return (
    <div className="mt-8">
      <div className="border p-4 my-4">
        <div className="flex gap-8">
          {/* 유저 기본정보 */}
          <div>
            <Image
              src={user.picture.large}
              alt={user.name.first}
              width={200}
              height={200}
            />
            <h2 className="text-xl font-bold">
              {user.name.title}. {user.name.first} {user.name.last}
            </h2>
            <p className="text-gray-600">{user.email}</p>

            <div className="mt-4">
              <p>
                <span className="font-bold">전화번호 : </span>
                {user.phone}
              </p>
              <p>
                <span className="font-bold">휴대전화번호 : </span>
                {user.cell}
              </p>
              <p>
                <span className="font-bold">사는 곳 : </span>
                {user.location.city}, {user.location.country}
              </p>
              <p>
                <span className="font-bold">등록일자 : </span>
                {new Date(user.registered.date).toLocaleDateString()}
              </p>

              <p>
                <span className="font-bold">생년월일 : </span>
                {new Date(user.dob.date).toLocaleDateString()}
              </p>
            </div>
          </div>

          {/* 지도영역 */}
          <iframe
            src={`https://maps.google.com/maps?q=${user.location.coordinates.longitude},${user.location.coordinates.latitude}&z=15&output=embed`}
            height="450"
            width="600"
          ></iframe>
        </div>
      </div>
    </div>
  );
};

export default SSR;
```

<br>

## 5.5 CSR

> fetch시, 요청이 있을 때 마다 지속해서 갱신해주면 CSR<br>
> "use client" 들어가면 CSR이다.

client side rendering이기 때문에, loading에 관련된 state 제어를 통해 사용자에게 알려줄 수 있다!(가져와야하는 데이터가 많을 때 유용할 듯)

<br>

"use client" 옵션 주기

```tsx
"use client"; // 옵션 부여

import Image from "next/image";
import React, { useEffect, useState } from "react";

import type { RandomUser } from "@/types";

const CSR = () => {
  const [user, setUser] = useState<RandomUser | null>(null);

  useEffect(() => {
    const fetchUser = async () => {
      const response = await fetch(`https://randomuser.me/api`);
      const { results } = await response.json();
      setUser(results[0]);
    };

    fetchUser();
  }, []);

  if (!user) {
    return <div>로딩중...</div>;
  }

  return (
    <div className="mt-8">
      <div className="border p-4 my-4">
        <div className="flex gap-8">
          {/* 유저 기본정보 */}
          <div>
            <Image
              src={user.picture.large}
              alt={user.name.first}
              width={200}
              height={200}
            />
            <h2 className="text-xl font-bold">
              {user.name.title}. {user.name.first} {user.name.last}
            </h2>
            <p className="text-gray-600">{user.email}</p>

            <div className="mt-4">
              <p>
                <span className="font-bold">전화번호 : </span>
                {user.phone}
              </p>
              <p>
                <span className="font-bold">휴대전화번호 : </span>
                {user.cell}
              </p>
              <p>
                <span className="font-bold">사는 곳 : </span>
                {user.location.city}, {user.location.country}
              </p>
              <p>
                <span className="font-bold">등록일자 : </span>
                {new Date(user.registered.date).toLocaleDateString()}
              </p>

              <p>
                <span className="font-bold">생년월일 : </span>
                {new Date(user.dob.date).toLocaleDateString()}
              </p>
            </div>
          </div>

          {/* 지도영역 */}
          <iframe
            src={`https://maps.google.com/maps?q=${user.location.coordinates.longitude},${user.location.coordinates.latitude}&z=15&output=embed`}
            height="450"
            width="600"
          ></iframe>
        </div>
      </div>
    </div>
  );
};

export default CSR;
```

<br><br>

# 6. Hydration

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
