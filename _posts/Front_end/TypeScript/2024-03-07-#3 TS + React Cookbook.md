---
title: "[TS] TS + React Cookbook"
categories: [TypeSciprt]
tag: [TypeSciprt]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 함수에서 TS 사용하기

> 함수에서 type을 지정하는 방법은 두 가지가 있다.

1. 사용하고자 하는 인자의 타입을 콜론(:)을 사용하여 작성한다.
2. 객체를 받고 함수의 반환값의 타입은 콜론(:) 다음에 명시한다.

```ts
function sum(a: number, b: number): number {
  return a + b;
}

function objSum({ a, b }: { a: number; b: number }): string {
  return `${a + b}`;
}
```

<br><br>

# 2. 비동기 함수에서 TS 사용하기

## 2.1 json 서버 열기

먼저 json 서버를 설치해주자

```js
yarn add json-server
```

<br>

그 후, db.json 파일을 만들어 아래와 같은 내용을 넣어주자

```js
{
  "people": [
    { "id": "1", "age": "25", "height": 179 },
    { "id": "2", "age": "22", "height": 163 },
    { "id": "3", "age": "29", "height": 184 }
  ]
}
```

<br>

아래 명령을 통해 json-sever를 실행하자

```js
yarn json-server --watch db.json --port 3001
```

<br>

## 2.2 비동기 함수와 TS

> 아래 비동기 함수를 보자

```ts
async function getPost() {
  const res = await fetch(`http://localhost:3001/people`);
  if (!res.ok) {
    throw new Error();
  }
  return res.json();
}
getPost().then((res) => console.log(res));
```

아래 명령어로 실행하기!

```
ts-node 파일명.ts
```

![](/assets/images/2024/2024-03-07-17-59-10.png)

res보면 any 타입인 것을 확인할 수 있다. any 타입은 모든 타입을 허용하기 때문에 (=치트키)TypeScript가 제공하는 강력한 정적 타입 검사 기능을 이용할 수 없게 된다.

<br>

> any 타입을 피하고 타입을 명시적으로 정의하여 타입 안정성을 보장하도록 해보자.

함수를 선언하는 부분에 `Promise<Person[]>` 작성하기

```ts
type Person = { id: number; age: number; height: number };

// getperson이 반환하는 건 person의 array
// 비동기 함수는 promise를 반환하기 때문에 promise 타입을 사용해야함
async function getperson(): Promise<Person[]> {
  const res = await fetch("http://localhost:3001/people");
  if (!res.ok) {
    throw new Error();
  }
  return res.json();
}

getperson().then((res) => console.log(res[0].age));
```

<br>

이 외에 return 부분에 type을 작성해줘도 함수의 type이 return부의 type으로 자동으로 추론되기 때문에 아래와 같이 작성해도 된다! (근데 Promise가 더 간단한 듯..)

```ts
type Person = { id: number; age: number; height: number };

async function getperson() {
  const res = await fetch("http://localhost:3001/people");
  if (!res.ok) {
    throw new Error();
  }
  return res.json() as any as Person[];
}

getperson().then((res) => console.log(res[0].age));
```

<br><br>

# 3 CRA를 TS로 만들기

TS 템플릿으로 React가 설치가 된다.

```
npx create-react-app my-first-ts-app --template typescript
```

![](/assets/images/2024/2024-03-07-19-58-42.png)

<br><br>

# 4. Generic <T> 간단히 이해하기

> 제네릭(generic)이란 데이터의 타입을 일반화한다(generalize)하는 방법을 의미한다.

간단히 데이터의 타입을 변수화 하는 것이다! 아래와 같이 사용한다.

> 제네릭 타입 선언 / 호출

```ts
type Generic<T> = {
  someValue: T;
};

type Test = Generic<string>;
```

<br>

> 제네릭 함수 선언 / 호출

```ts
function someFunc<T>(value: T) {}
someFunc<string>("hello");
```

즉, 타입을 생성할 때 원하는 타입을 인자로 받아 넣어주면 된다.

<br><br>

# 5. useState에서 사용하기

```ts
import React, { useState } from "react";

const App = () => {
  // 명시적으로 제네릭에 원하는 type을 넣어주면 된다.
  const [counter, setCounter] = useState<string>("hello");

  return <div>{counter}</div>;
};

export default App;
```

<br>

초기 값이 없으면 undefined가 나온다!
![](/assets/images/2024/2024-03-07-21-24-48.png)

<br>

<br>

# 6. Passing Props

<br>

# 7. Children Props

<br>

# 8. Generic, Utility Type 통해서 Props용 Type 만들기

<br>

# 9. Event Handler 사용하기

<br>
