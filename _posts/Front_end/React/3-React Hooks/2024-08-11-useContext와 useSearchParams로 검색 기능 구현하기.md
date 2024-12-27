---
title: "[React] useContext와 useSearchParams로 검색 기능 구현하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 기존 useSearchParams 코드

> useSearchParams로 구현된 검색 기능을 useContext로 바꿔보자

검색어를 URL 쿼리 파라미터로 관리하고, 쿼리 파라미터가 변경될 때마다 데이터를 필터링한다.

```json
[
  {
    "id": 1,
    "title": "내가 제일 좋아하는 아이스크림은?",
    "content": "부라보콘 피스타치오 맛!!!🍦🍦"
  },
  {
    "id": 2,
    "title": "내가 제일 좋아하는 음식은?",
    "content": "돈가스!! 💰💰💲"
  },
  {
    "id": 3,
    "title": "테스트",
    "content": "A B C d e f"
  }
]
```

```jsx
import { useEffect, useState } from "react";
import { useSearchParams } from "react-router-dom";
import dummyData from "dummy.json";

const Search = () => {
  const [searchParams, setSearchParams] = useSearchParams(); // 쿼리 파라미터 상태

  /* searchParams.get("search")을 초기 상태로 넣어주는 이유?
    사용자가 페이지를 새로 고침하거나 URL을 직접 입력했을 때 검색 쿼리 파라미터를 기억하기 위함! */
  const [query, setQuery] = useState(searchParams.get("search") || ""); // 검색 쿼리 상태
  const [filteredData, setFilteredData] = useState(dummyData); // 필터링된 데이터 상태

  // 검색 버튼 클릭 시 검색어를 URL 쿼리 파라미터로 설정
  const handleSearch = (e) => {
    e.preventDefault(); // 폼 제출 시 페이지 리로드 방지
    const trimmedQuery = query.trim(); // 검색어의 공백 제거
    setSearchParams({ search: trimmedQuery }); // 쿼리 파라미터 업데이트
  };

  // 검색어가 변경될 때 필터링된 데이터 업데이트
  useEffect(() => {
    const searchParam = searchParams.get("search");
    const toLowerCaseParams = searchParam ? searchParam.toLowerCase() : "";

    if (toLowerCaseParams) {
      const filteredDummy = dummyData.filter(
        (dummy) =>
          dummy.title.toLowerCase().includes(toLowerCaseParams) ||
          dummy.content.toLowerCase().includes(toLowerCaseParams)
      );
      setFilteredData(filteredDummy);
    } else {
      setFilteredData(dummyData); // 검색어가 공백만 있는 경우 전체 데이터 표시
    }
  }, [searchParams]); // searchParams가 변경될 때마다 호출됨

  return (
    <>
      <form onSubmit={handleSearch}>
        <input
          type="text"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          placeholder="검색어를 입력하세요."
        />
        <button type="submit">검색하기</button>
      </form>
      <div>
        {filteredData.length > 0 ? (
          filteredData.map((dummy) => (
            <ul key={dummy.id}>
              <li>{dummy.title}</li>
              <li>{dummy.content}</li>
            </ul>
          ))
        ) : (
          <p>검색 결과가 없습니다.</p>
        )}
      </div>
    </>
  );
};

export default Search;
```

<br><br>

# 2. useContext로 검색 기능 구현하기

## 2.1 Context 생성

> 이제 위 코드를 useContext를 사용해 검색 기능을 구현해보자

먼저, 검색 상태를 관리하기 위한 Context를 생성하자

{% raw %}

```jsx
// src/context/SearchContext.jsx
import { createContext, useEffect, useState } from "react";
import { useSearchParams } from "react-router-dom";
import dummyData from "dummy.json";

// Context 생성
export const SearchContext = createContext();

// Context Provider
export const SearchContextProvider = ({ children }) => {
  const [searchParams, setSearchParams] = useSearchParams(); // 쿼리 파라미터 상태
  const [query, setQuery] = useState(searchParams.get("search") || ""); // 검색 쿼리 상태
  const [filteredData, setFilteredData] = useState(dummyData); // 필터링된 데이터 상태

  const handleSubmit = (e) => {
    e.preventDefault(); // 폼 제출 시 페이지 리로드 방지
    const trimmedQuery = query.trim(); // 검색어의 공백 제거
    setSearchParams({ search: trimmedQuery }); // 쿼리 파라미터 업데이트
  };

  // 검색어가 변경될 때 필터링된 데이터 업데이트
  useEffect(() => {
    const searchParam = searchParams.get("search");
    const toLowerCaseParams = searchParam ? searchParam.toLowerCase() : "";

    if (toLowerCaseParams) {
      const filteredDummy = dummyData.filter(
        (dummy) =>
          dummy.title.toLowerCase().includes(toLowerCaseParams) ||
          dummy.content.toLowerCase().includes(toLowerCaseParams)
      );
      setFilteredData(filteredDummy);
    } else {
      setFilteredData(dummyData); // 검색어가 공백만 있는 경우 전체 데이터 표시
    }
  }, [searchParams]);

  return (
    <SearchContext.Provider
      value={{ query, setQuery, handleSubmit, filteredData }}
    >
      {children}
    </SearchContext.Provider>
  );
};
```

{% endraw %}

<br>

## 2.2 Context Provider 설정

생성한 SearchProvider를 애플리케이션의 루트 또는 필요한 컴포넌트 범위에 적용하여 Context가 전역적으로 사용될 수 있도록 하자.

```jsx
// src/HomePage.jsx
import Search from "components/ContextPractice/Search";
import { SearchContextProvider } from "context/SearchContext";

const HomePage = () => {
  return (
    <SearchContextProvider>
      <Search />
    </SearchContextProvider>
  );
};

export default HomePage;
```

<br>

## 2.3 Search.jsx에서 Context 사용하기

이제 Search 컴포넌트에서 useContext를 사용하여 검색 기능을 구현할 수 있다.

```jsx
// src/components/SearchForm.jsx
iimport { useContext } from 'react';
import { SearchContext } from 'context/SearchContext';
const Search = () => {
  const { query, setQuery, handleSubmit, filteredData } = useContext(SearchContext);

  return (
    <>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          placeholder="검색어를 입력하세요."
        />
        <button type="submit">검색하기</button>
      </form>
      <div>
        {filteredData.length > 0 ? (
          filteredData.map((dummy) => (
            <ul key={dummy.id}>
              <li>{dummy.title}</li>
              <li>{dummy.content}</li>
            </ul>
          ))
        ) : (
          <p>검색 결과가 없습니다.</p>
        )}
      </div>
    </>
  );
};

export default Search;
```

<br>
