---
title: "[Project] Vanilla.js + TMDB API를 사용한 영화 소개 및 검색 사이트 만들기"
categories: [Project]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

![Ciné Search](../../../assets/images/2024/%EB%85%B9%ED%99%94_2024_01_10_13_08_22_89.gif)

<br>

---

<br>

# 1. 프로젝트 개요

## 1. 프로젝트 내용

"Vanilla.js + TMDB API를 사용한 영화 소개 및 검색 사이트" 입니다.

<br>

## 1.2 구현 항목

① TMDB 오픈 API를 사용하여 인기영화 데이터를 가져옵니다.

② TMDB에서 받아온 데이터를 브라우저 화면에 카드 형태의 데이터로 보여줍니다.

③ 카드에 마우스를 갖다대면 카드가 뒤집히면서 영화 줄거리와 버튼이 나타납니다.

④ 버튼 클릭시 클릭한 영화 id 를 나타내는 alert 창을 띄웁니다.

⑤ 영화 제목을 검색하여 찾을 수 있도록 합니다.

<br><br>

# 2. TMDB API 사용하기

## 2.1 API KEY 발급하기

TMDB 오픈 API 사용하려면 회원가입을 해야합니다.

TMDB 회원 가입과 API KEY 발급에 대한 설명은 [[sagein 님 블로그]](https://www.sagein.net/703)를 참조합시다!

회원가입이 끝났으면 [[TMDB API 공식 문서]](https://developer.themoviedb.org/reference/intro/getting-started)에 접속해 주세요.

<br>

## 2.2 TMDB API로부터 데이터 받아오기

인기 영화 데이터를 가져올 것이므로 MOVIE LISTS > Popular를 선택해주세요.

그 후 링크를 복사해주세요.

![](/assets/images/2024/2024-01-06-02-44-01.png)

복사한 링크뒤에 <span style="color:indianred">?api_key=[발급받은 API KEY]</span>를 입력해주세요.

<span style="color:LightPink">&language=ko-KR</span>을 입력하면 한국어로 번역됩니다!

→ https://api.themoviedb.org/3/tv/popular<span style="color:indianred">?api_key=[발급받은 API KEY]</span><span style="color:LightPink">&language=ko-KR</span> 이 되겠네요!

<br>

한번 접속해 볼까요?

Key:Value로 이루어진 JSON 형태로 결과를 받는 것을 확인할 수 있어요!

![](/assets/images/2024/2024-01-06-02-56-40.png)

<br><br>

# 3. TMDB API로 카드 생성하기

## 3.1 HTML, CSS 구성하기

먼저 html과 css로 생성하고자 하는 카드를 만들어줬어요.

CSS 코드는 제 [[깃허브 - main.css, basc.css, reset.css]](https://github.com/MyNameSieun/Cin-Search.git)에서 확인하실 수 있어요!

```html
<div class="container">
  <div class="flip">
    <!-- <div class="card-container">
          <div class="card-front">
            <img
              src="./images/라라랜드 포스터_세로.jpg"
              class="card-img-top"
              alt="영화 포스터"
            />
            <div class="card-body">
              <div class="card-info">
                <h5 class="card-title">라라랜드</h5>
                <p class="star">⭐4.5</p>
              </div>
            </div>
          </div>
          <div class="card-back">
            <div class="card-body">
              <div class="card-info">
                <h5 class="card-title">라라랜드</h5>
                <p class="star">⭐4.5</p>
              </div>
              <hr class="hr-top" />
              <p class="card-text">
                헐리우드 배우지망생 미아와 재즈바를 차리는게 꿈인 피아노 연주자
                세바스찬은 우연히 만나 서로의 꿈을 응원하는 연인 사이가 된다.
                세바스찬은 오디션 낙방에 아쉬워하는 미아를 격려하고, 돈을 벌기
                위해 공연 투어에 나서지만 미아는 재즈바라는 꿈을 본격적으로 쫓지
                않는 그가 탐탁지 않다. 한편, 미아에게도 좋은 기회가 찾아오고
                둘의 관계는 새로운 단계를 맞이한다.
              </p>
              <hr class="hr-bottom" />
              <a class="btn btn-primary">영화 id</a>
            </div>
          </div>
        </div> -->
  </div>
</div>
```

왜 주석처리를 해놨을까요?

바로 주석처리해놓은 부분을 동적으로 생성해주기 위함이에요!

TMDB API로부터 데이터를 가져와 js fetch 함수를 사용해 카드의 내용을 변경해줄거에요.

<br>

예를 들어볼까요?

html의 `<h5 class="card-title">라라랜드</h5>`에서 영화 제목을 동적으로 생성해주기 위해 js의 fetch 함수로 TMDB API을 영화 제목 데이터를 받아와 `<h5 class="card-title">${movie.title}</h5>` 이런 식으로 동적으로 생성할 수 있는거죠!

<br>

## 3.2 fetch 함수로 API 호출하기

### 3.2.1 fetch 함수 사용하기

이제 HTML에 주석처리한 부분을 TMDB API로부터 데이터를 가져와 동적으로 생성하는 작업을 수행해봅시다!

fetch 함수를 사용하면 서버에서 데이터를 가져올 수 있어요.

> 먼저 콘솔에다 가져온 데이터를 출력해봅시다.

```js
// movies.js
const api_key = "발급받은 API KEY";
const url = `https://api.themoviedb.org/3/movie/popular?api_key=${api_key}&language=ko-KR`;

// 데이터 가져오기
return fetch(url)
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

잘 찍히네요!
![](/assets/images/2024/2024-01-07-16-18-42.png)

<br>

이 데이터들 중에 영화 검색 사이트에 사용할 항목은 다음과 같습니다.

![](/assets/images/2024/2024-01-08-23-34-43.png)

|        항목        |    데이터    |
| :----------------: | :----------: |
|      영화 id       |      id      |
|        제목        |    title     |
|     내용 요약      |   overview   |
| 포스터 이미지 경로 | poster_path  |
|        평점        | vote_average |

<br>

### 3.2.2 showMovies()

> `showMovies()`함수는 API를 사용하여 인기 있는 영화 목록을 가져오는 역할을 합니다. fetch() 함수를 사용하여 API에 요청을 보내고, 응답을 JSON 형식으로 변환한 후에 data.results에서 영화 목록을 추출합니다. 그리고 displayMovies() 함수를 호출하여 영화 목록을 화면에 표시합니다.

이제 콘솔이 아닌 웹 페이지에다 데이터를 출력해봅시다.

이전에는 `.then((data) => console.log(data))`를 사용해서 데이터를 콘솔에 출력했었는데, <br>
이제는 `displayMovies(movies)`라는 함수를 받은 것을 확인할 수 있어요!

```js
function showMovies() {
  // movies.js
  const api_key = "발급받은 API KEY";
  const url = `https://api.themoviedb.org/3/movie/popular?api_key=${api_key}&language=ko-KR`;

  fetch(url)
    .then((response) => response.json())
    .then((data) => {
      const movies = data.results; //  API로부터 받아온 데이터를 movies 변수에 저장
      displayMovies(movies); // -> 배열
    })
    .catch((error) => {
      console.error("데이터를 가져오는 중 오류 발생:", error);
    });
}
```

<br>

### 3.2.3 generateCardHtml(movie)

> `generateCardHtml(movie)`함수는 영화 정보를 받아와서 해당 영화의 카드 HTML을 생성하는 역할을 합니다. movie 객체에서 필요한 정보(제목, 평점, 포스터 이미지 등)를 추출하여 HTML 문자열로 만들고, 최종적으로 `<div>` 요소로 변환하여 반환하는 함수입니다.

```js
// movies.js
function generateCardHtml(movie) {
  const overview = movie.overview || "영화 정보가 없습니다.";
  const html = `
    <div class="card-front">
       <img src="https://image.tmdb.org/t/p/w500${
         movie.poster_path
       }" class="card-img-top" alt="${movie.title} 포스터" />
       <div class="card-body">
         <div class="card-info">
           <h5 class="card-title">${movie.title}</h5>
           <p class="star">⭐ ${movie.vote_average.toFixed(2)}</p>
         </div>
       </div>
     </div>
     <div class="card-back">
       <div class="card-body">
       <div class="card-info">
         <h5 class="card-title">${movie.title}</h5>
         <p class="star">⭐ ${movie.vote_average.toFixed(2)}</p>
       </div>
       <hr class="hr-top" />
       <p class="card-text">
         ${overview}
       </p>
       <hr class="hr-bottom" />

       <a class="btn btn-primary" href="details.html">상세 정보</a>
     </div>
   </div>
   `;

  // 새로운 <div> 요소를 생성
  const el = document.createElement("div");
  // 생성한 <div> 요소에 "card-container" 클래스를 추가
  el.classList.add("card-container");
  // 생성한 <div> 요소의 내부 HTML을 설정
  el.innerHTML = html;
  // 생성한 문자열 반환
  return el;
}
```

<br>

### 3.2.4 displayMovies(movieList)

> `displayMovies(movieList)`함수는 영화 목록을 받아와서 각 영화 카드를 화면에 표시하는 역할을 합니다. movieList 배열을 순회하면서 각 영화에 대해 generateCardHtml() 함수를 호출하여 카드를 생성하고, 생성된 카드를 화면에 추가합니다.

```js
// movies.js
function displayMovies(movieList) {
  // 설정한 내부 HTML을 가진 card_container를 flip 요소의 자식 요소로 추가
  //  나중에 생성된 영화 카드를 추가할 위치를 참조하는 역할
  const flip = document.querySelector(".flip");

  movieList.forEach((movie) => {
    const el = generateCardHtml(movie);
    flip.appendChild(el);
  });
}
```

<br>

### 3.2.5 export & import

> 위의 함수들은 export 키워드를 사용하여 다른 파일에서도 사용할 수 있도록 모듈로 내보내줬습니다!

```js
// movies.js
export { showMovies, generateCardHtml, displayMovies };
```

import로 내보낸 모듈을 가져와야겠죠?

```js
import { showMovies } from "./movies.js";
showMovies();
```

<br><br>

# 4. 기능 추가

## 4.1 버튼 클릭시 영화 id 출력

카드 뒷면에 있는 버튼 클릭시 영화 id를 출력해봅시다!<br>`flip.appendChild(el);`아래다가 작성해줄게요.<br>

두 가지의 방법으로 구현할 수 있어요!

① querySelector 사용

```js
// movies.js
const movieIdBtn = card_container.querySelector(".btn");
movieIdBtn.addEventListener("click", function () {
  const movieId = movie.id;
  alert(`영화 id : ${movieId}`);
});
```

<br>

② querySelectorAll 사용

```js
// movies.js
const movieIdBtnList = card_container.querySelectorAll(".btn");
movieIdBtnList.forEach(function (btn) {
  btn.addEventListener("click", function () {
    const movieId = movie.id;
    alert(`영화 id : ${movieId}`);
  });
});
```

<br>

querySelectorAll은 뭐가 많이 복잡해보이죠? <br>
저는 여기서 헤맸었답니다... 고민의 흔적이 보이시나요..??😹

![](/assets/images/2024/2024-01-08-23-52-22.png)

<br>

querySelectorAll은 요소를 NodeList로 반환한다는 사실을 잊고 있었어요!<br>
즉, querySelectorAll을 사용하려면 forEach와 같은 배열 메서드를 사용해서 요소(btn) 하나하나에 접근해야합니다!

![](/assets/images/2024/2024-01-08-23-59-48.png)

<br>

## 4.2 사용자 검색 기능

사용자가 검색어를 입력하면 그에 맞는 영화 카드를 보여주는 함수를 정의해줬어요.

모든 카드들을 순회하면서 제목에 검색어가 포함되면 보이고, 포함안되면 안보이게 구현했습니다!

search.js 파일을 생성해서 작성해줬습니다.

```js
// search.js
export function searchMovies() {
  const searchForm = document.querySelector(".form-control");

  // 사용자가 입력을 할 때마다 이벤트가 실행
  searchForm.addEventListener("input", function () {
    // this는 searchForm 요소를 참조
    // toLowerCase: 입력된 텍스트를 소문자로 변환
    const searchKeyword = this.value.toLowerCase();

    // movieCards의 각 요소에 대해 함수를 실행
    const movieCards = document.querySelectorAll(".card-container");
    movieCards.forEach((card) => {
      // 카드의 제목을 소문자로 변환하여 title 변수에 할당
      const title = card.querySelector(".card-title").textContent.toLowerCase();

      // title이 searchKeyword를 포함한다면 cardDisplay는 "block", 그렇지 않다면 "none"
      const cardDisplay = title.includes(searchKeyword) ? "block" : "none";
      card.style.display = cardDisplay;

      // 아래와 같은 코드
      //   if (title.includes(searchKeyword)) {
      //     card.style.display = "block";
      //   } else {
      //     card.style.display = "none";
      //   }
    });
  });
}
```

<br>

export를 해줬으니 import로 모듈을 가져옵시다!

```js
// main.js
import { searchMovies } from "./search.js";
import { showMovies } from "./movies.js";
showMovies();
searchMovies();
```

<br>

## 4.2 form 태그 reload 현상 방지

form 안에 있는 button 태그의 기본 type은 submit 이며, 버튼 클릭 시 submit 이벤트가 발생하여 폼이 제출될 때 새로고침됩니다.

`event.preventDefault()` 메서드는 이러한 폼 제출 기본 동작을 취소하는 역할을 합니다. 이 메서드를 사용하면 폼이 제출될 때 발생하는 페이지 새로고침을 방지할 수 있습니다!

```js
// main.js
document
  .querySelector("#inputForm")
  .addEventListener("submit", function (event) {
    event.preventDefault();
  });
```

<br><br>

# 5. 전체 코드

## 5.1 index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Ciné Search</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"
      crossorigin="anonymous"
    />
    <link rel="stylesheet" href="./style/reset.css" />
    <link rel="stylesheet" href="./style/base.css" />
    <link rel="stylesheet" href="./style/style.css" />
    <script type="module" src="./src/main.js"></script>
  </head>
  <body>
    <main class="container">
      <!--navbar-->
      <nav class="navbar">
        <div class="container-fluid">
          <a class="navbar-brand" href="./index.html">Ciné Search</a>
          <form id="inputForm" class="d-flex" role="search">
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search"
              aria-label="Search"
              autofocus
            />
            <button class="btn" type="submit">Search</button>
          </form>
        </div>
      </nav>

      <!--trailer-container-->
      <figure class="trailer-container">
        <video
          playsinline
          controls
          poster="./assets/images/라라랜드 포스터_가로.jpg"
        >
          <source src="./assets/video/라라랜드 트레일러.mp4" type="video/mp4" />
        </video>
      </figure>

      <!--card-->
      <section>
        <div class="flip"></div>
      </section>
    </main>

    <!--footer-->
    <footer class="footer"></footer>
    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

<br>

## 5.2 main.js

```js
// main.js
import { searchMovies } from "./search.js";
import { showMovies } from "./movies.js";
showMovies();
searchMovies();

// form 태그 reload 현상 방지
document
  .querySelector("#inputForm")
  .addEventListener("submit", function (event) {
    event.preventDefault();
  });
```

<br>

## 5.3 movies.js

```js
// movies.js
function showMovies() {
  const api_key = "[발급받은 API KEY]";
  const url = `https://api.themoviedb.org/3/movie/popular?api_key=${api_key}&language=ko-KR`;

  fetch(url)
    .then((response) => response.json())
    .then((data) => {
      const movies = data.results; //  API로부터 받아온 데이터를 movies 변수에 저장
      displayMovies(movies); // -> 배열
    })
    .catch((error) => {
      console.error("데이터를 가져오는 중 오류 발생:", error);
    });
}

// 영화 정보를 받아서 카드 HTML을 생성하는 함수
function generateCardHtml(movie) {
  const overview = movie.overview || "영화 정보가 없습니다.";
  const html = `
    <div class="card-front">
       <img src="https://image.tmdb.org/t/p/w500${
         movie.poster_path
       }" class="card-img-top" alt="${movie.title} 포스터" />
       <div class="card-body">
         <div class="card-info">
           <h5 class="card-title">${movie.title}</h5>
           <p class="star">⭐ ${movie.vote_average.toFixed(2)}</p>
         </div>
       </div>
     </div>
     <div class="card-back">
       <div class="card-body">
       <div class="card-info">
         <h5 class="card-title">${movie.title}</h5>
         <p class="star">⭐ ${movie.vote_average.toFixed(2)}</p>
       </div>
       <hr class="hr-top" />
       <p class="card-text">
         ${overview}
       </p>
       <hr class="hr-bottom" />
  
       <a class="btn btn-primary" href="details.html">상세 정보</a>
     </div>
   </div>
   `;

  // 새로운 <div> 요소를 생성
  const el = document.createElement("div");
  // 생성한 <div> 요소에 "card-container" 클래스를 추가
  el.classList.add("card-container");
  // 생성한 <div> 요소의 내부 HTML을 설정
  el.innerHTML = html;

  return el;
}

// 영화 목록을 받아서 각 영화 카드를 화면에 표시하는 함수
function displayMovies(movieList) {
  // 설정한 내부 HTML을 가진 card_container를 flip 요소의 자식 요소로 추가

  //  나중에 생성된 영화 카드를 추가할 위치를 참조하는 역할
  const flip = document.querySelector(".flip");

  movieList.forEach((movie) => {
    const el = generateCardHtml(movie);
    flip.appendChild(el);
  });
}

export { showMovies, generateCardHtml, displayMovies };
```

<br>

## 5.4 search.js

```js
// search.js
export function searchMovies() {
  const searchForm = document.querySelector(".form-control");

  // 사용자가 입력을 할 때마다 이벤트가 실행
  searchForm.addEventListener("input", function () {
    // this는 searchForm 요소를 참조
    // toLowerCase: 입력된 텍스트를 소문자로 변환
    const searchKeyword = this.value.toLowerCase();

    // movieCards의 각 요소에 대해 함수를 실행
    const movieCards = document.querySelectorAll(".card-container");
    movieCards.forEach((card) => {
      // 카드의 제목을 소문자로 변환하여 title 변수에 할당
      const title = card.querySelector(".card-title").textContent.toLowerCase();

      // title이 searchKeyword를 포함한다면 cardDisplay는 "block", 그렇지 않다면 "none"
      const cardDisplay = title.includes(searchKeyword) ? "block" : "none";
      card.style.display = cardDisplay;

      // 아래와 같은 코드
      //   if (title.includes(searchKeyword)) {
      //     card.style.display = "block";
      //   } else {
      //     card.style.display = "none";
      //   }
    });
  });
}
```
