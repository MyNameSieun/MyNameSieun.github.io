---
title: "[ToyProject] 바닐라 JS와 TMDB API 사용해 영화 검색 사이트 만들기"
categories: [ToyProject]
tag: [ToyProject, HTML, JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

잠깐!🤚
처음 오픈 API를 사용하면서의 시행 착오를 담은 포스팅입니다!

다소 난잡한 글이 될지라도 양해 바랍니다😹
{: .notice--warning}

<br>

# 1. 프로젝트 개요

## 1. 프로젝트 설명

"Vanilla.js + TMDB API를 사용한 영화 소개 및 검색 사이트" 입니다.

<br>

## 1.2 구현 기능

① TMDB 오픈 API를 사용하여 인기영화 데이터를 가져옵니다.

② TMDB에서 받아온 데이터를 브라우저 화면에 카드 형태의 데이터로 보여줍니다.

③ 카드에 마우스를 가져다대면 카드가 뒤집히면서 영화 줄거리와 버튼이 나타납니다.

④ 버튼 클릭시 클릭한 영화 id 를 나타내는 alert 창을 띄웁니다.

⑤ 영화 제목을 검색하여 찾을 수 있도록 합니다.

<br><br>

# 2. TMDB 오픈 API 사용하기

## 2.1 API KEY 발급

TMDB 오픈 API 사용하려면 회원가입을 해야합니다.

TMDB 회원 가입과 API KEY 발급에 대한 설명은 [[sagein 님 블로그]](https://www.sagein.net/703)를 참조합시다!

회원가입이 끝났으면 [[TMDB API 공식 문서]](https://developer.themoviedb.org/reference/intro/getting-started)에 접속해 주세요.

<br>

## 2.2 데이터 가져오기

인기 영화 데이터를 가져올 것이므로 MOVIE LISTS를 Popular를 선택해주세요.

그 후 링크를 복사해주세요.

![](/assets/images/2024/2024-01-06-02-44-01.png)

복사한 링크뒤에 <span style="color:indianred">?api_key=<<발급받은 API KEY>></span>를 입력해주세요.

<span style="color:LightPink">&language=ko-KR</span>을 입력하면 한국어로 번역됩니다!

> https://api.themoviedb.org/3/tv/popular<span style="color:indianred">?api_key=<<발급받은 API KEY>></span><span style="color:LightPink">&language=ko-KR</span> 이 되겠네요!

<br>

한번 접속해 볼까요?

Key:Value로 이루어진 JSON 형태로 결과를 받는 것을 확인할 수 있어요!

![](/assets/images/2024/2024-01-06-02-56-40.png)

<br><br>

## 2.2 fetch() 함수로 데이터 가공하기

우선 콘솔에다 가져온 데이터를 출력해봅시다!
fetch로

```js
const url =
  "https://api.themoviedb.org/3/tv/popular?api_key=13b14dad7e58423573b90a27c47ebfbf&language=ko-KR";

// 데이터 가져오기
return fetch(url)
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

잘 찍히네요!
![](/assets/images/2024/2024-01-07-16-18-42.png)

<br>

이 데이터들 중에 필요한 항목만 가져올 수 있도록 데이터를 가공할 거에요.

사용할 항목은 다음과 같아요.

|        항목        |    데이터    |
| :----------------: | :----------: |
|      영화 id       |      id      |
|        제목        |    title     |
|     내용 요약      |   overview   |
| 포스터 이미지 경로 | poster_path  |
|        평점        | vote_average |

> 카드에 데이터 띄우기

```js
// Fetch API 사용하여 데이터 띄우기
const movieContainer = document.querySelector("#movieContainer");

const url =
  "https://api.themoviedb.org/3/movie/popular?api_key=13b14dad7e58423573b90a27c47ebfbf&language=ko-KR";

fetch(url)
  .then((response) => response.json())
  .then((data) => {
    const movies = data.results;

    movies.forEach((movie) => {
      // card_front
      const card_front = document.createElement("div");
      card_front.classList.add("card-front");

      card_front.innerHTML = `
        <img src="https://image.tmdb.org/t/p/w500${movie.poster_path}" class="card-img-top" alt="${movie.title} 포스터" />
        <div class="card-body">
          <div class="card-info">
            <h5 class="card-title">${movie.title}</h5>
            <p class="star">⭐ ${movie.vote_average}</p>
          </div>
        </div>
      `;

      movieContainer.appendChild(card_front);

      // card_back
      const card_back = document.createElement("div");
      card_back.classList.add("card-back");

      card_back.innerHTML = `
      <div class="card-body">
      <div class="card-info">
        <h5 class="card-title">${movie.title}</h5>
        <p class="star">⭐ ${movie.vote_average}</p>
      </div>
      <hr class="hr-top" />
      <p class="card-text">
      ${movie.overview}
      </p>
      <hr class="hr-bottom" />

      <a class="btn btn-primary">영화 id</a>
    </div>
      `;

      movieContainer.appendChild(card_back);
    });
  })

  .catch((error) => {
    console.error("데이터를 가져오는 중 오류 발생:", error);
  });
```

<br>

> 검색 기능 추가

폼에 이벤트 리스너를 추가하여 폼 제출을 처리합니다.
이벤트 리스너 내에서 사용자가 입력한 검색어 값을 가져옵니다.
영화 카드를 반복하며 영화 제목이 입력한 키워드를 포함하는지 확인합니다.
검색 결과에 따라 카드를 표시하거나 숨깁니다.

```js
// Fetch API를 사용하여 데이터 가져오기
const movieContainer = document.querySelector("#movieContainer");

const url =
  "https://api.themoviedb.org/3/movie/popular?api_key=13b14dad7e58423573b90a27c47ebfbf&language=ko-KR";

let allMovies = []; // 초기에 모든 영화를 저장하기 위한 배열

fetch(url)
  .then((response) => response.json())
  .then((data) => {
    allMovies = data.results;

    // 초기에 모든 영화 표시
    displayMovies(allMovies);

    const searchForm = document.querySelector("form");

    searchForm.addEventListener("submit", function (event) {
      event.preventDefault(); // 폼 제출 기본 동작 방지

      const searchInput = document.querySelector("input[type=search]");
      const keyword = searchInput.value.trim().toLowerCase();

      // 검색 결과에 따라 영화 카드를 표시하거나 숨김
      const filteredMovies = allMovies.filter((movie) =>
        movie.title.toLowerCase().includes(keyword)
      );

      displayMovies(filteredMovies);
    });
  })
  .catch((error) => {
    console.error("데이터를 가져오는 중 오류 발생:", error);
  });

// 영화 카드 표시 함수
function displayMovies(movies) {
  movieContainer.innerHTML = ""; // 기존 카드 삭제

  movies.forEach((movie) => {
    // card-front 생성 및 표시
    const card_front = document.createElement("div");
    card_front.classList.add("card-front");

    card_front.innerHTML = `
      <img src="https://image.tmdb.org/t/p/w500${movie.poster_path}" class="card-img-top" alt="${movie.title} 포스터" />
      <div class="card-body">
        <div class="card-info">
          <h5 class="card-title">${movie.title}</h5>
          <p class="star">⭐ ${movie.vote_average}</p>
        </div>
      </div>
    `;

    movieContainer.appendChild(card_front);

    // card-back 생성 및 표시
    const card_back = document.createElement("div");
    card_back.classList.add("card-back");

    card_back.innerHTML = `
      <div class="card-body">
        <div class="card-info">
          <h5 class="card-title">${movie.title}</h5>
          <p class="star">⭐ ${movie.vote_average}</p>
        </div>
        <hr class="hr-top" />
        <p class="card-text">
          ${movie.overview}
        </p>
        <hr class="hr-bottom" />

        <a class="btn btn-primary">영화 id</a>
      </div>
    `;

    movieContainer.appendChild(card_back);
  });
}
```
