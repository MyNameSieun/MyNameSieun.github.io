---
title: "[React] react-quill 사용하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 개요

## 1.1 react-quill 개념

> react-quill은 React에서 사용 가능한 Quill 편집기(HTML WYSIWYG 편집기)를 위한 라이브러리이다.

[[Quill 공식문서↗️]](https://quilljs.com/docs/quickstart)

<br>

## 1.2 설치

```shell
yarn add react-quill quill
```

<br>

## 1.3 기본 사용방법

> ReactQuill 컴포넌트를 기본적으로 사용하는 방법은 다음과 같다.

```jsx
import { useState } from "react";
import ReactQuill from "react-quill";
import "react-quill/dist/quill.snow.css";

const TextEditor = () => {
  const [value, setValue] = useState("");

  return (
    <div>
      <ReactQuill theme="snow" value={value} onChange={setValue} />
    </div>
  );
};

export default TextEditor;
```

![](/assets/images/2024/2024-08-31-07-22-35.png)

<br><br>

# 3. React-Quill 커스터마이징

- 퀼은 다양한 커스터마이징 옵션을 제공한다. 툴바 설정, 에디터 테마, 모듈 등을 설정할 수 있다.
- <span style="color:indianred">⚠️</span> createGlobalStyle을 사용하여 전역 스타일을 설정할 때, Quill의 기본 스타일이 덮어쓰일 수 있다. 이로 인해 Quill의 기본 스타일이나 툴바가 예상대로 작동하지 않을 수 있으므로, 덮어 쓸만한 코드를 주석처리 하거나 삭제해 주도록 하자.

<br>

## 3.1 테마 설정

> 퀼 에디터는 `snow`, `bubble` 테마를 제공한다.

```jsx
import "react-quill/dist/quill.snow.css"; // 기본 테마
// 또는
import "react-quill/dist/quill.bubble.css"; // 버블 테마

<ReactQuill value={value} onChange={handleChange} theme="snow" />;
```

<br>

## 3.2 툴바 설정

에디터의 툴바에서 사용할 항목을 설정할 수 있다. 예를 들어, 굵게, 기울임, 리스트, 링크 등을 포함한 툴바를 설정할 수 있다.

> 방법 1: ReactQuill의 modules 프로퍼티를 사용하여 툴바를 직접 설정

{% raw %}

```jsx
// 툴바 설정
const modules = {
  toolbar: [
    [{ header: [1, 2, 3, 4, 5, false] }], // [{ header: '1' }, { header: '2' }],
    ["bold", "italic", "underline", "strike", "blockquote"],
    ["link", "image", "video"],
    [{ list: "ordered" }, { list: "bullet" }],
    [{ indent: "-1" }, { indent: "+1" }],
    ["clean"],
  ],
};

<ReactQuill
  theme="snow"
  value={value}
  onChange={setValue}
  // 정의한 modules를 ReactQuill 컴포넌트에 전달
  modules={modules}
  style={{ height: "300px" }}
/>;
```

{% endraw %}

<br>

> 방법 2: 커스텀 툴바 컴포넌트 사용

{% raw %}

```jsx
// styles/CustomToolbar
export const CustomToolbar = () => (
  <div id="toolbar">
    <span className="ql-formats">
      <select className="ql-size" defaultValue="medium">
        <option value="small">Small</option>
        <option value="medium">Medium</option>
        <option value="large">Large</option>
        <option value="huge">Huge</option>
      </select>
      <select className="ql-header" defaultValue="">
        <option value="">Normal</option>
        <option value="1">Header 1</option>
        <option value="2">Header 2</option>
        <option value="3">Header 3</option>
        <option value="4">Header 4</option>
        <option value="5">Header 5</option>
        <option value="6">Header 6</option>
      </select>
    </span>
    <span className="ql-formats">
      <button className="ql-bold" />
      <button className="ql-italic" />
      <button className="ql-underline" />
      <button className="ql-strike" />
      <button className="ql-blockquote" />
    </span>
    <span className="ql-formats">
      <select className="ql-color" />
      <select className="ql-background" />
    </span>
    <span className="ql-formats">
      <button className="ql-image" />
      <button className="ql-video" />
    </span>
    <span className="ql-formats">
      <button className="ql-clean" />
    </span>
  </div>
);
```

{% endraw %}

{% raw %}

```jsx
...
import { CustomToolbar } from "styles/CustomToolbar";

const TextEditor = ({ value, onChange }) => {
  const [value, setValue] = useState("");

  // 툴바 설정
  const modules = {
    toolbar: {
      container: "#toolbar", // CustomToolbar의 id
    },
  };

  return (
    <div>
      <CustomToolbar />
      <ReactQuill
        theme="snow"
        modules={modules}
        value={value}
        onChange={onChange}
      />
    </div>
  );
};

export default TextEditor;
```

{% endraw %}

<br>

## 3.3 포맷 설정

> 포맷 설정을 사용하여 퀼 에디터에서 사용자가 어떤 텍스트 포맷팅을 사용할 수 있을지를 결정할 수 있다.

{% raw %}

```jsx
// 포맷 설정
const formats = [
  "header",
  "font",
  "size",
  "bold",
  "italic",
  "underline",
  "strike",
  "blockquote",
  "list",
  "bullet",
  "indent",
  "link",
  "image",
  "video",
  "color",
  "background",
];

<ReactQuill
  value={value}
  onChange={handleChange}
  modules={modules}
  // 정의한 formats을 ReactQuill 컴포넌트에 전달
  formats={formats}
/>;
```

{% endraw %}

<br><br>

# 4. 게시글 작성하기

## 4.1 CustomToolbar.jsx

```jsx
export const CustomToolbar = () => (
  <div id="toolbar">
    <span className="ql-formats">
      <select className="ql-size" defaultValue="medium">
        <option value="small">Small</option>
        <option value="medium">Medium</option>
        <option value="large">Large</option>
        <option value="huge">Huge</option>
      </select>
      <select className="ql-header" defaultValue="">
        <option value="">Normal</option>
        <option value="1">Header 1</option>
        <option value="2">Header 2</option>
        <option value="3">Header 3</option>
        <option value="4">Header 4</option>
        <option value="5">Header 5</option>
        <option value="6">Header 6</option>
      </select>
    </span>
    <span className="ql-formats">
      <button className="ql-bold" />
      <button className="ql-italic" />
      <button className="ql-underline" />
      <button className="ql-strike" />
      <button className="ql-blockquote" />
    </span>
    <span className="ql-formats">
      <select className="ql-color" />
      <select className="ql-background" />
    </span>
    <span className="ql-formats">
      <button className="ql-image" />
      <button className="ql-video" />
    </span>
    <span className="ql-formats">
      <button className="ql-clean" />
    </span>
  </div>
);
```

<br>

## 5.2 TextEditor.jsx

{% raw %}

```jsx
// src/components/TextEditor
import ReactQuill from "react-quill";
import "react-quill/dist/quill.snow.css";
import { CustomToolbar } from "styles/CustomToolbar";

const TextEditor = ({ value, onChange }) => {
  // 툴바 설정
  const modules = {
    toolbar: {
      container: "#toolbar", // CustomToolbar의 id
    },
  };

  // 포맷 설정
  const formats = [
    "header",
    "font",
    "size",
    "bold",
    "italic",
    "underline",
    "strike",
    "blockquote",
    "list",
    "bullet",
    "indent",
    "link",
    "image",
    "video",
    "color",
    "background",
  ];

  return (
    <div>
      <CustomToolbar />
      <ReactQuill
        theme="snow"
        modules={modules}
        formats={formats}
        value={value}
        onChange={onChange}
        style={{ height: "70vh" }}
      />
    </div>
  );
};

export default TextEditor;
```

{% endraw %}

<br>

## 5.3 PostFormPage.jsx

{% raw %}

```jsx
// src/pages/public/PostFormPage.jsx
import { useState } from "react";
import { useNavigate } from "react-router-dom";
import { createPost } from "api/posts";
import TextEditor from "../../../src/components/TextEditor";

const PostFormPage = () => {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");

  const navigate = useNavigate();

  const handleSubmit = async (e) => {
    e.preventDefault();

    try {
      await createPost({ title, content });
      alert("게시물 작성이 완료되었습니다!");
      navigate("/post-list");
    } catch (error) {
      console.error("Error creating post:", error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="title">제목</label>
        <input
          type="text"
          id="title"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="content">내용</label>
        <TextEditor value={content} onChange={setContent} />
      </div>
      <button type="submit">작성 완료</button>
    </form>
  );
};

export default PostFormPage;
```

{% endraw %}

<br><br>

# 6. 게시글 출력하기

## 6.1 dangerouslySetInnerHTML와 XSS 위험

> 아래처럼 에디터를 통해 작성한 글은 html 태그 형태로 생성이 되기 때문에, 서버나 사용자로부터 받은 HTML 문자열을 직접 렌더링해야 해야한다. 이때, `dangerouselySetInnerHTML`을 사용한다.

![](/assets/images/2024/2024-08-31-08-27-06.png)

{% raw %}

```jsx
<div dangerouslySetInnerHTML={{ __html: post.content }} />
```

{% endraw %}

<br>

dangerouslySetInnerHTML는 JSX에서 HTML을 직접 삽입할 수 있는 유일한 방법이다.

하지만 만약 post.content에 악의적인 스크립트가 포함되어 있다면, 이 스크립트가 실행되어 XSS 공격에 노출될 수 있기 때문에 이 방법은 위험할 수 있다.

<br>

## 6.2 dompurify의 사용

> 이러한 XSS 위험을 방지하기 위해 DOMPurify를 사용한다.

- DOMPurify는 HTML을 안전하게 출력하기 위해 사용하는 라이브러리이다.
- DOMPurify는 입력된 HTML 문자열을 "정화(sanitize)"하여 악성 스크립트를 제거한다.
- 이 과정에서 XSS(교차 사이트 스크립팅) 공격에 사용될 수 있는 요소와 속성을 제거하여 안전한 HTML만 남기게 된다.

<br>

> dompurify를 적용해보자

① 먼저 dompurify를 설치해주자

```shell
yarn add dompurify @types/dompurify
```

② 그리고 아래처럼 post.content를 `DOMPurify.sanitize`로 감싸서 정화(sanitize)된 HTML만 출력하도록 하면 된다.

{% raw %}

```jsx
<div
  dangerouslySetInnerHTML={{
    __html: DOMPurify.sanitize(post.content),
  }}
/>
```

{% endraw %}

➡️ 이렇게 하면 post.content에 악성 스크립트가 포함되어 있더라도, DOMPurify가 이를 제거하므로, 최종적으로 렌더링되는 HTML은 안전하게 사용할 수 있다.

<br>

> 실제 코드에 적용해보자!

{% raw %}

```jsx
// src/pages/public/PostListPage.jsx
import { fetchPosts } from "api/posts";
import { useEffect, useState } from "react";
import { useNavigate } from "react-router-dom";
import styled from "styled-components";
import "react-quill/dist/quill.snow.css";
import DOMPurify from "dompurify"; // dompurify import

const PostListPage = () => {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const navigate = useNavigate();

  useEffect(() => {
    const loadPosts = async () => {
      try {
        const response = await fetchPosts();
        setPosts(response.data);
      } catch (error) {
        console.error(error);
      } finally {
        setLoading(false);
      }
    };
    loadPosts();
  }, []);

  if (loading) {
    return <p>로딩 중...</p>;
  }

  return (
    <div>
      {posts.length > 0 ? (
        <ul>
          {posts.map((post) => (
            <StPostItem
              key={post.id}
              onClick={() => navigate(`/posts/${post.id}`)}
            >
              <h3>제목: {post.title}</h3>
              <div
                dangerouslySetInnerHTML={{
                  __html: DOMPurify.sanitize(post.content),
                }}
                style={{
                  marginTop: "30px",
                  overflow: "hidden",
                  whiteSpace: "pre-wrap",
                }}
              />
              <p>작성일: {post.createdAt}</p>
            </StPostItem>
          ))}
        </ul>
      ) : (
        <p>등록된 글이 없습니다.</p>
      )}
    </div>
  );
};

export default PostListPage;

const StPostItem = styled.li`
  padding: 1rem;
  border: 1px solid black;
  cursor: pointer;
`;
```

{% endraw %}

![](/assets/images/2024/2024-08-31-14-39-15.png)

<br>
