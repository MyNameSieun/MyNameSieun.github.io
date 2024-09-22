---
title: "[TS] 폼 데이터 관리하기 - FormData vs useState"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. FormData

## 1.1 FormData 개요

> `FormData`는 브라우저에서 제공하는 내장 객체로, 폼(form) 요소에 포함된 데이터를 손쉽게 가져오고 다룰 수 있게 해준다.

- `<form>` 요소 내의 입력 필드 데이터를 자동으로 수집한다.
- 각 필드의 `name`을 key로, 입력된 값을 value으로 저장한다.
- 텍스트뿐만 아니라 파일, 이미지 등도 처리할 수 있다.

<br>

## 1.2 FormData 사용 예시

> `axios`를 사용해 `FormData`로 서버에 요청을 보내 데이터를 처리하고 싶을 때 [[HTTP Content Type - form data 사용↗️] ](https://mynamesieun.github.io/javascript/HTTP-Content-Type/#322-multipartform-data-%EC%82%AC%EC%9A%A9)포스팅을 참고하자-!

- FormData 생성
  - `new FormData(e.currentTarget)`는 폼 데이터 객체를 만든다.
  - 여기서 `e.currentTarget`은 `<form>` 요소를 가리키므로, `<form>` 내부의 모든 필드를 자동으로 처리할 수 있다.<br><br>
- 폼 필드의 데이터 가져오기:
  - `formData.get("title")`는 `<input name="title">` 필드의 값을 가져온다.
  - 이 때 반환되는 값은 `string | File | null`이므로, 값이 존재하고 텍스트 데이터임을 확신하기 위해 `as string`으로 타입 단언을 사용한다.<br><br>
- 타입 단언
  - `as string`을 사용해 `formData.get("title")`의 결과가 반드시 `string`임을 컴파일러에게 알려준다.
  - 이게 없다면 타입스크립트는 반환값이 `FormDataEntryValue` 타입(string 또는 File)일 수도 있다고 경고한다.

```tsx
// src/components/todo/TodoForm.tsx

import { useTodoStore } from "../../store/todoStore";

const TodoForm = () => {
  const { addTodo } = useTodoStore();

  const handleOnSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();

    // FormData 객체를 생성하여 form 데이터를 가져옴
    const formData = new FormData(e.currentTarget);

    const form = e.currentTarget;

    // formData 객체에서 입력 필드 값 가져오기
    const title = formData.get("title") as string;
    const content = formData.get("content") as string;

    // 처리 로직 구현
    const nextTodo = {
      id: crypto.randomUUID(),
      title,
      content,
      isDone: false,
      deadline: new Date().toLocaleDateString(),
    };

    addTodo(nextTodo);
    alert("추가 완료!");

    // 폼을 초기화
    form.reset();
  };

  return (
    <form onSubmit={handleOnSubmit}>
      <div>
        <label htmlFor="title">제목: </label>
        <input id="title" name="title" />
      </div>
      <div>
        <label htmlFor="content">내용: </label>
        <input id="content" name="content" />
      </div>
      <button type="submit">제출</button>
    </form>
  );
};

export default TodoForm;
```

<br>

# 1.3 FormData 메서드

> set이 append 메서드와 다른 점은 set은 name과 동일한 이름을 가진 필드를 모두 제거하고 새로운 필드 하나를 추가한다는 데 있다.

따라서 set 메서드를 쓰면 name을 가진 필드가 단 한 개만 있게끔 보장할 수 있다.

| 메서드                                  | 설명                                                                                   |
| --------------------------------------- | -------------------------------------------------------------------------------------- |
| `formData.append(name, value)`          | `name`과 `value`를 가진 폼 필드를 추가                                                 |
| `formData.append(name, blob, fileName)` | `<input type="file">` 형태의 필드를 추가. 세 번째 인수 `fileName`은 파일의 이름을 설정 |
| `formData.delete(name)`                 | `name`에 해당하는 필드를 삭제                                                          |
| `formData.get(name)`                    | `name`에 해당하는 필드의 값을 가져옴                                                   |
| `formData.has(name)`                    | `name`에 해당하는 필드가 존재하면 `true`, 없으면 `false`를 반환                        |
| `formData.set(name, value)`             | `name`을 가진 필드를 모두 제거하고 새로운 필드 하나를 추가                             |
| `formData.set(name, blob, fileName)`    | 동일한 `name` 필드를 모두 제거하고, 새로운 파일 필드를 추가                            |

<br><br>

# 2. useState

## 2.1 useState 개요

> 리액트에서 폼 데이터를 처리하는 또 다른 방법은 `useState`를 활용하는 것이다.

- 각 입력 필드에 대해 별도의 상태를 만들고, 입력 값이 바뀔 때마다 그 값을 상태로 관리한다.
- 폼의 각 입력 필드를 `value` 속성으로 상태와 연결하여 입력 값이 즉시 반영된다.

## 2.2 useState 사용 예시

```tsx
const [title, setTitle] = useState<string>("");
const [content, setContent] = useState<string>("");

const handleOnSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();

  // 상태에 저장된 값들을 이용해 처리 로직 구현
  console.log(title, content);
};

return (
  <form onSubmit={handleOnSubmit}>
    <input
      type="text"
      value={title}
      onChange={(e) => setTitle(e.target.value)}
    />
    <input
      type="text"
      value={content}
      onChange={(e) => setContent(e.target.value)}
    />
    <button type="submit">제출</button>
  </form>
);
```

<br><br>

# 3. 결론

➡️ 타입스크립트에서는 보통 `FormData`를 사용하여 폼 데이터를 관리한다-! 그 이유는 아래와 같다.

> ① 브라우저 API의 타입 지원

- `FormData`는 브라우저 내장 API로, TS가 잘 인식해서 어떤 값이 어떤 타입인지 쉽게 알 수 있다.
- `formData.get("title")`은 기본적으로 `string | File | null`이 될 수 있지만, 여기서 `title`이 반드시 문자열이어야 한다고 확신할 때 타입 단언을 사용해 타입스크립트가 `title`을 `string`으로 인식하게 만들어 주는 것이다.

```tsx
const formData = new FormData(e.currentTarget);
const title = formData.get("title") as string; // 타입 단언을 사용해 string으로 변환
```

<br>

> ② 파일 처리에 적합

- FormData는 텍스트 데이터뿐만 아니라 파일, 이미지와 같은 다양한 타입의 데이터를 지원한다.
- 타입스크립트에서 `FormData`를 사용하면, 특정 필드가 파일인지, 텍스트인지에 따라 처리 방식을 달리할 수 있으며, 이러한 동적 데이터를 다룰 때도 타입 안전성을 유지할 수 있다.

```tsx
const file = formData.get("file") as File;
```

<br>

> ③ 자동 데이터 수집

`FormData`는 폼에 있는 모든 데이터를 자동으로 수집해줘서, 일일이 상태로 관리할 필요 없이 데이터를 바로 가져올 수 있다.

<br>

> ④ 내장 타입 안정성

- 타입스크립트와 함께 `FormData`를 사용할 때는 타입스크립트의 내장적인 타입 검증을 통해 API 사용 오류를 줄일 수 있다.
- `formData.append()`, `formData.get()`과 같은 메서드는 특정 입력 타입을 강제하여 예상치 못한 오류를 방지한다.
- 예를 들어, `FormData.append()`는 `string | Blob` 타입의 값만 허용하므로, 잘못된 타입의 데이터를 추가하려고 할 때 타입스크립트가 경고를 해준다.

```tsx
formData.append("file", selectedFile); // Blob이나 File 객체를 추가
formData.append("name", userName); // string 값만 허용
```

<br><br>

# 참조

- [https://ko.javascript.info/formdata](https://ko.javascript.info/formdata)

<br>
