---
title: "[React] React Markdown Editor 사용하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

---

<br>

[@uiw/react-md-editor ↗️](https://github.com/uiwjs/react-md-editor)는 React로 Markdown 기반 애플리케이션을 구현할 때 사용하는 라이브러리이다.

<br>

---

<br>

# 1. 설치

```shell
yarn add @uiw/react-md-editor
```

<br><br>

# 2. 기본 사용법

![](/assets/images/2025/2025-01-21-05-27-00.png)

> 마크다운 편집 컴포넌트

```tsx
"use client";

import { useState } from "react";
import MDEditor from "@uiw/react-md-editor";
import Editor from "./components/Editor";

const MdViewPage = () => {
  const [contents, setContents] = useState("**Hello Markdown!**");

  return (
    <div className="h-screen flex flex-col gap-4 p-4">
      {/* Markdown Editor (Markdown 편집용) */}
      <Editor contents={contents} onChange={setContents} />

      {/* Rendered Markdown (Markdown 표시용)*/}
      <MDEditor.Markdown source={contents} />
    </div>
  );
};

export default MdViewPage;
```

<br>

> 마크다운 렌더링 컴포넌트

```tsx
"use client";

import MDEditor from "@uiw/react-md-editor";

interface EditorProps {
  contents: string;
  onChange: (value: string) => void;
}

const Editor = ({ contents, onChange }: EditorProps) => {
  return (
    <div>
      <MDEditor
        value={contents}
        onChange={(value) => onChange(value || "")}
        height={100 + "%"}
        autoFocus={false}
        visibleDragbar={true}
      />
    </div>
  );
};

export default Editor;
```

<br><br>

# 3. 주요 기능

## 3.1 MDEditor과 MDEditor.Markdown

| **컴포넌트**          | **설명**                                       |
| --------------------- | ---------------------------------------------- |
| **MDEditor**          | Markdown 텍스트를 작성할 수 있는 편집기를 제공 |
| **MDEditor.Markdown** | 작성된 Markdown 내용을 실시간으로 렌더링       |

<br>

## 3.2 주요 Props

| **Prop**           | **타입**   | **설명**                                            |
| ------------------ | ---------- | --------------------------------------------------- |
| **value**          | `string`   | 에디터의 초기 Markdown 내용                         |
| **onChange**       | `function` | 에디터의 내용이 변경될 때 호출되는 콜백             |
| **height**         | `number`   | 에디터 높이 (기본값: 200px)                         |
| **preview**        | `string`   | `"live"`, `"edit"`, `"preview"` 중 하나를 선택 가능 |
| **visibleDragbar** | `boolean`  | 에디터 높이를 조절할 수 있는 드래그바 표시 여부     |

<br><br>

# 4. Markdown Editor 커스터마이징

## 4.1 에디터 모드 선택

> preview prop을 사용해 에디터의 동작 모드를 설정할 수 있다.

- "`live"`: 실시간 미리보기 (기본값)
- `"edit"`: 편집 모드만 표시
- `"preview"`: 미리보기 모드만 표시

```tsx
<MDEditor value={value} onChange={setValue} preview="edit" />
<MDEditor value={value} onChange={setValue} preview="preview" />
```

<br>

## 4.2 높이 조절

> `height`와 `visibleDragbar`를 설정하면 에디터 높이를 조절할 수 있다.

```tsx
<MDEditor
  value={value}
  onChange={setValue}
  height={400}
  visibleDragbar={true}
/>
```

<br>
