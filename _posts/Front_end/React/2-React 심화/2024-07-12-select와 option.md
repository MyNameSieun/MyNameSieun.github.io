---
title: "[React] select와 option"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. select

> 기능

사용자에게 선택 가능한 옵션 목록을 드롭다운 형식으로 제공

> 사용법

`<select>` 태그 안에 여러 `<option>` 태그를 넣어 각각의 선택 가능한 항목을 정의

> 속성

`multiple`: 여러 옵션을 선택 가능

<br>

# 2. option

> 기능

`<select>` 내에서 개별 선택 항목을 정의

> 속성

- `value`: 폼 데이터와 함께 전송될 옵션의 실제 값을 지정 (option 내부에 적은 건 그냥 보여주기 용)
- `selected`: 해당 `<option>`이 기본 선택 값

> 예시 코드

```js
const [sortOrder, setSortOrder] = useState("asc");

/* 정렬 옵션 선택 드롭다운 메뉴 */
<select value={sortOrder} onChange={(e) => setSortOrder(e.target.value)}>
  <option value="asc">오름차순</option>
  <option value="desc">내림차순</option>
</select>;
```

<br>

# 3. 사용 예시

```jsx
import { useState } from "react";

const TodoSort = ({ setTodos }) => {
  const [sortOrder, setSortOrder] = useState("asc");

  const onChangeSortOrder = (e) => {
    const newSortOrder = e.target.value; // 사용자가 선택한 값
    setSortOrder(newSortOrder); // 상태를 업데이트

    setTodos((prev) =>
      [...prev].sort((a, b) => {
        const dataA = new Date(a.deadline);
        const dateB = new Date(b.deadline);

        // 오름차순 정렬
        if (newSortOrder === "asc") {
          return dataA - dateB;
        }
        //내림차순 정렬
        else {
          return dateB - dataA;
        }
      })
    );
  };

  return (
    <select value={sortOrder} onChange={onChangeSortOrder}>
      <option value="asc" selected>
        오름차순
      </option>
      <option value="desc">내림차순</option>
    </select>
  );
};

export default TodoSort;
```

![](/assets/images/2024/2024-07-12-20-03-15.png)

<br>
