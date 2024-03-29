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

> 명시적으로 제네릭에 원하는 type을 넣어주면 된다.

```tsx
import React, { useState } from "react";

const App = () => {
  const [counter, setCounter] = useState<string>("hello");

  return <div>{counter}</div>;
};

export default App;
```

<br>

초기 값이 없으면 undefined가 나온다!

![](/assets/images/2024/2024-03-07-21-24-48.png)

<br>

> 버튼 클릭시 counter가 증가하도록 해보자

```tsx
import React, { useState } from "react";

const App = () => {
  // 명시적으로 제네릭에 원하는 type을 넣어주면 된다.
  const [counter, setCounter] = useState<number>(1);
  const increment = () => {
    setCounter((prev) => 1);
  };
  return <div onClick={increment}>{counter}</div>;
};

export default App;
```

아래처럼 type이 일치하지 않는 경우 제한을 해주는 것을 볼 수 있다.

![](/assets/images/2024/2024-03-08-12-15-47.png)

<br><br>

# 6. Passing Props

> 원래 js에서는 props를 아래와 같이 사용했다.

```tsx
import React, { useState } from "react";

const Parent = () => {
  const [count, setCount] = useState();
  return <Child count={count}></Child>;
};

const Child = ({ count }) => {
  return <div>{count}</div>;
};

export default Parent;
```

하지만 위 코드는 count 가 암묵적으로 any타입을 가져 에러가 발생하게 된다.

![](/assets/images/2024/2024-03-08-14-56-36.png)

<br>

> 따라서 아래와 같이 타입을 써줘야한다.

```tsx
import React, { useState } from "react";

const Parent = () => {
  const [count, setCount] = useState("");
  return <Child count={count}></Child>;
};

const Child = ({ count }: { count: string }) => {
  return <div>{count}</div>;
};

export default Parent;
```

<br>

> 만약 아래와 같이 여러 개의 props를 전달 받는 상황이면 어떻게 될까? → 코드가 길어지고 가독성이 떨어진다.

```tsx
const Child = ({
  count,
  double,
  id,
  des,
}: {
  count: string;
  double: string;
  id: string;
  des: string;
}) => {
  return <div>{count}</div>;
};
```

<br>

> 위 문제를 해결하기 위해 type 부분만 선언하고, 선언한 type을 컴포넌트에서 props로 받도록 사용할 수 있다.

```tsx
type Props = {
  count: string;
  double: string;
  id: string;
  des: string;
};

const Child = ({ count, double, id, des }: Props) => {
  return <div>{count}</div>;
};
```

<br>

> todolist에서 setTodos를 Todo 컴포넌트에다 넘기는 예제를 살펴보자.

```tsx
import React, { useState } from "react";

type Todo = {
  id: string;
  idDone: boolean;
};
const App = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  return (
    <>
      {todos.map(({ id }) => (
        <Todo key={id} />
      ))}
    </>
  );
};
function Todo() {
  return <div></div>;
}

export default App;
```

<br>

> 콜백 함수를 props로 넘겨줄 때는 아래와 같이 작성해주면 된다.

```tsx
import React, { useState } from "react";

type Todo = {
  id: string;
  idDone: boolean;
};
const App = () => {
  const [todos, setTodos] = useState<Todo[]>([]);
  return (
    <>
      {todos.map(({ id }) => (
        <Todo key={id} id={id} setTodos={setTodos} />
      ))}
    </>
  );
};
function Todo({
  id,
  setTodos,
}: {
  id: string;
  setTodos: React.Dispatch<React.SetStateAction<Todo[]>>;
}) {
  const deleteTodo = () => {
    setTodos((prev) => prev.filter((todo) => todo.id !== id));
  };

  return <div onClick={deleteTodo}></div>;
}

export default App;
```

![](/assets/images/2024/2024-03-08-17-14-12.png)

<br>

`React.Dispatch<React.SetStateAction<Todo[]>>`는 아래와 같이 축약해서 쓸 수 있다.

`setTodos: (cb: (todo: Todo[]) => Todo[]) => void;`

```tsx
function Todo({
  id,
  setTodos,
}: {
  id: string;
  setTodos: (cb: (todo: Todo[]) => Todo[]) => void;
}) {
  const deleteTodo = () => {
    setTodos((prev) => prev.filter((todo) => todo.id !== id));
  };
```

Todo를 받아 Todo를 반환하는 콜백을 받으면 void 함수(아무 리턴 값이 없는 함수)를 setTodos에 넣어준다는 의미이다.

<br>

> 아래처럼 부모 컴포넌트에서 자식 컴포넌트에 props로 함수를 넘겨줄 때 매개변수, 리턴부로 타입을 나눠 작성하여 type을 물려줄 수 있다.

```tsx
import React, { useState } from "react";

type Todo = {
  id: string;
  idDone: boolean;
};
const App = () => {
  const [todos, setTodos] = useState<Todo[]>([]);

  const deleteTodo = (id: string) => {
    const newTodos = todos.filter((todo) => todo.id === id);
    setTodos(newTodos);
  };
  return (
    <>
      {todos.map(({ id }) => (
        <Todo key={id} id={id} deleteTodo={deleteTodo} />
      ))}
    </>
  );
};

function Todo({
  id,
  deleteTodo,
}: {
  id: string;
  deleteTodo: (id: string) => void;
}) {
  const handeOnClick = () => {
    deleteTodo(id);
  };
  return <div onClick={handeOnClick}></div>;
}

export default App;
```

<br><br>

# 7. Children Props

> 방법 1) PropsWithChildren 사용하기

BaseType을 제네릭에 넣어주기만 하면 바로 children props를 사용할 수 있다.

```tsx
import React, { PropsWithChildren } from "react";

type BaseType = {
  id: string;
};

function Child({ children }: PropsWithChildren<BaseType>) {
  return <div>{children}</div>;
}

export function Parent() {
  return (
    <Child id="">
      <div>children</div>
    </Child>
  );
}
```

<br>

하지만, 위 방법은 children을 넘기지 않더라도 아무런 오류를 뱉지 않는다.(옵셔널 체이닝을 사용하기 때문) → PropsWithChildren type은 엄격하지 않다고 볼 수 있다.(하지만 많이 사용)

![](/assets/images/2024/2024-03-09-00-22-26.png)

<br>

> 방법 2) PropsWithChildrenrn과 비슷한 StrictChildren같은 type을 만들어줌으로써 해결이 가능하다.

옵셔널이 아닌 무조건 type+children을 사용하게 하여 더 엄격하게 type을 사용할 수 있게 한다.

```tsx
import React, { ReactNode } from "react";

type BaseType = {
  id: string;
};

type StrictChildren<T> = T & { children: ReactNode };

function Child({ children }: StrictChildren<BaseType>) {
  return <div>{children}</div>;
}

export function Parent() {
  return (
    <Child id="">
      <div>children</div>
    </Child>
  );
}
```

엄격하게 type을 지정하여 children을 지웠을 때 바로 오류가 나는 것을 볼 수 있다.

![](/assets/images/2024/2024-03-09-00-28-28.png)

<br><br>

# 8. Event Handler 사용하기

> 이벤트 핸들러에서 TS를 사용할 때는 어떻게 해야할까?

```tsx
import React, { useState } from "react";

const App = () => {
  const [counter, setCounter] = useState<number>(1);
  const increment = (e) => {
    e.target.value;
  };

  return <div onClick={increment}>{counter}</div>;
};

export default App;
```

event에 대한 type을 지정해야한다.

![](/assets/images/2024/2024-03-09-13-09-06.png)

<br>

> onClick 메서드와 같은 자주 사용하는 메서드들의 type을 지정하는 방법을 알아보자.

방법 1) 함수를 작성하고 event 넣은 후 마우스 올려보기

```tsx
return <div onClick={(e) => {}}>{counter}</div>;
```

어떤 type을 썼는지 타입 추론이 가능하다.

![](/assets/images/2024/2024-03-09-13-13-18.png)

<br>

type 복사 붙여넣기!

```tsx
import React, { useState } from "react";

const App = () => {
  const [counter, setCounter] = useState<number>(1);
  const increment = (e: React.MouseEvent<HTMLDivElement, MouseEvent>) => {
    e.target;
  };

  return <div onClick={increment}>{counter}</div>;
};

export default App;
```

<br><br>

방법 2) 더 간단한 방법이다. 메서드에다 호버링할 때 나오는 `...EventHandler`의 `...`를 통해 type을 추론할 수 있다.

onClick일 경우

```tsx
import React, { MouseEvent, useState } from "react";

const App = () => {
  const [counter, setCounter] = useState<number>(1);
  const increment = (e: MouseEvent<HTMLDivElement>) => {
    e.target;
  };

  return <div onClick={increment}>{counter}</div>;
};

export default App;
```

![](/assets/images/2024/2024-03-09-13-19-55.png)

<br>

onChange일 경우

```tsx
import React, { FormEvent, useState } from "react";

const App = () => {
  const [counter, setCounter] = useState<number>(1);
  const increment = (e: FormEvent<HTMLDivElement>) => {
    e.target;
  };

  return <div onChange={increment}>{counter}</div>;
};

export default App;
```

![](/assets/images/2024/2024-03-09-13-21-33.png)

<br>
