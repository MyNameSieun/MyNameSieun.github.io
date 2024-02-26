---
title: "[React P.J, ForYou] Redux-Toolkit 적용하기 #8"
categories: [Project]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

《 [redux 적용하기 #7](https://mynamesieun.github.io/project/7-redux-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/) 》에 이어 작성하는 글입니다.
{: .notice--danger}

지난 포스팅 《 [redux 적용하기 #7](https://mynamesieun.github.io/project/7-redux-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/) 》 에서 관리중이던 모든 state를 redux-tookit 으로 관리해보자.

<br>

# 1. 초기 세팅

> 설치하기

```js
yarn add @reduxjs/toolkit
```

<br>

> Redux-Toolkit에서는 module을 Slice라는 개념을 이용해서 모듈을 구현하고 있다. 따라서 파일 명을 아래와 같이 바꿔주자

- 기존 파일명 : letter.js, activeTab.js
- 수정 파일명 : letterSlice.js, activeTabSlice.js

![](/assets/images/2024/2024-02-27-02-08-41.png)

<br><br>

# 2. Config Store 설정

> Redux-Toolkit에서는 createStore 대신 configStore를 사용한다. 각 모듈을 combineReducers로 통합하여 스토어를 설정한다.

configStore 안에 reducer를 정의해주면 된다.

```js
// src > store > config > configStore
import letters from "store/modules/letterSlice";
import activeTab from "store/modules/activeTabSlice";

import { combineReducers, configureStore } from "@reduxjs/toolkit";

const rootReducer = combineReducers({ letters, activeTab });

const store = configureStore({ reducer: rootReducer });

export default store;
```

<br><br>

# 3. 리듀서 함수 작성하기

## 3.1 activeTabSlice 모듈

기존 redux로 설정된 activeTabSlice 모듈을 Redux-Toolkit을 사용한 코드로 변경해보자

```js
// src > store > modules > activeTabSlice.js
import { createSlice } from "@reduxjs/toolkit";

const initialState = "토토로";

const activeTabSlice = createSlice({
  name: "activeTab",
  initialState,
  reducers: {
    setActiveTab: (state, action) => {
      const activeTab = action.payload;
      return activeTab;
    },
  },
});

export const { setActiveTab } = activeTabSlice.actions; // 리액트 컴포넌트에서 action creator 사용하기 위해 export
export default activeTabSlice.reducer; // store에서 리듀서를 가져오기 위해 export
```

<br>

## 3.2 letterSlice 모듈

기존 redux로 설정된 letterSlice 모듈을 Redux-Toolkit을 사용한 코드로 변경해보자

```js
// src > store > modules > letterSlice.js
import { createSlice } from "@reduxjs/toolkit";
import fakeData from "fakeData.json";

const initialState = fakeData;

const letterSlice = createSlice({
  name: "letter",
  initialState,
  reducers: {
    addLetter: (state, action) => {
      const newLetter = action.payload;
      return [newLetter, ...state];
    },
    deleteLetter: (state, action) => {
      const letterId = action.payload;
      return state.filter((letter) => letter.id !== letterId);
    },
    editLetter: (state, action) => {
      const { id, editingText } = action.payload;
      return state.map((letter) => {
        if (letter.id === id) {
          return { ...letter, content: editingText };
        }
        return letter;
      });
    },
  },
});

export const { addLetter, deleteLetter, editLetter } = letterSlice.actions; // 리액트 컴포넌트에서 action creator 사용하기 위해 export
export default letterSlice.reducer; // store에서 리듀서를 가져오기 위해 export
```

<br>
