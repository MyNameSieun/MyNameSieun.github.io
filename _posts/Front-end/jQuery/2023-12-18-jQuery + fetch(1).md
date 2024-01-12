---
title: "[jQuery] jQuery + fetch(1)"
categories: [jQuery]
tag: [jQuery]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ fetch

## ▷ fetch란?

> - 예전 포스팅[ 《 [JS] 5주차 QUIZ 》](https://velog.io/@sieunpark/5%EC%A3%BC%EC%B0%A8-QUIZ) 에서 잠깐 fetch에 대해서 배웠다.

- 이때, 'fetch'는 "Promise를 반환하는 메서드" 라고 했었는데,
  간단히 말하면, 'fetch'는 <span style="color:CornflowerBlue">인터넷을 통해 데이터를 요청하고 받아오는 과정</span>을 의미한다.

fetch를 사용하면 공공데이터를 프로젝트에 활용할 수 있다!! 아주 재밌겠다. 한번 알아보자

<br>

- 먼저 Fetch 연습을 위해 아래 코드를 붙여넣어보자.

```jsx
<!doctype html>
<html lang="ko">

<head>
    <meta charset="UTF-8">
    <title>Fetch 시작하기</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script>
        function hey() {
            alert('연결완료');
        }
    </script>
</head>

<body>
    <button onclick="hey()">fetch 연습!</button>
</body>

</html>
```

<br>

- 미세먼지 OpenAPI를 hey() 메서드 안에 아래와 같이 추가해보자

```jsx
function hey() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";
}
```

<br>

- 그 후, 메서드 아래에 Fetch를 추가해보자
  Fetch 기본 골격은 아래과 같다.

```jsx
fetch("여기에 URL을 입력") // 이 URL로 웹 통신을 요청한다. 괄호 안에 다른 것이 없다면 GET!
  .then((res) => res.json()) // 통신 요청을 받은 데이터는 res라는 이름으로 JSON화 한다.
  .then((data) => {
    console.log(data); // 개발자 도구에 찍어보기
  }); // JSON 형태로 바뀐 데이터를 data라는 이름으로 붙여 사용한다.
```

<br>

> Fetch 코드 설명

- `fetch("여기에 URL을 입력")` ← 이 URL로 웹 통신 요청을 보낼 거야!
  - 괄호 안에 **URL밖에 들어있지 않다면** `기본상태인 GET!`
  - `.then()` ← 통신 요청을 받은 다음 이렇게 할 거야!
  - `res ⇒ res.json()`
    - ← 통신 요청을 받은 데이터는 res 라는 이름을 붙일 거야(변경 가능)
    - ← res는 JSON 형태로 바꿔서 조작할 수 있게 할 거야!
  - `.then(data ⇒ {})` ←JSON 형태로 바뀐 데이터를 data 라는 이름으로 붙일 거야

<br>

> 리마인드🗨️

- GET 요청은, url뒤에 아래와 같이 붙여서 데이터를 가져간다.
  - http://naver.com?param=value&param2=value2
- POST 요청은, data : {} 에 넣어서 데이터를 가져간다.
  - data: { param: 'value', param2: 'value2' },

<br>

- 함 콘솔에 찍히는지 보까?

```jsx
    <script>
      function hey() {
        let url = "http://spartacodingclub.shop/sparta_api/seoulair";

        fetch(url)
          .then((res) => res.json())
          .then((data) => {
            console.log(data);
          });
      }
    </script>
```

<br>

- 짜잔!🎇 잘 찍힌당
  ![](https://velog.velcdn.com/images/sieunpark/post/cb5e67be-f183-4ccd-a16c-e33e31cacd3a/image.png)

<br>

➡️ 즉, OpenAPI의 JSON들이 데이터라는 변수 안에 저장된 것이라는 것을 알 수 있다!

<br>

# ▶ fetch 연습

## ▷ 미세먼지 OpenAPI

> 미세먼지 OpenAPI 데이터를 다뤄보자.

1. 미세먼지 데이터가 어디에 있는지 찾기 ➡️ RealtimeCityAir > row 에 미세먼지 데이터가 들어있다.
   ![](https://velog.velcdn.com/images/sieunpark/post/953fa327-9c91-4a3c-9304-02e10472aa72/image.png)

<br>

2. 미세먼지 데이터 꺼내기

```jsx
function hey() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      console.log(data["RealtimeCityAir"]["row"][0]);
      // RealtimeCityAir의 row의 0번째 값
    });
}
```

<br>

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/b98b75b5-c434-41bb-8a06-526f10ecdde8/image.png)

<br>

## ▷ 반복문 사용

> 반복문으로 구 데이터를 출력해보자.

- row는 아래 그림에서 보이는 것처럼 list로 구성되어있다.
  ![](https://velog.velcdn.com/images/sieunpark/post/3855d7a0-e4ee-40e7-bdf9-03e25a4d591a/image.png)

<br>

- 그렇기때문에 row에 해당하는 list를 rows로 받아온다음, 반복문으로 데이터를 출력할 수 있다!

```jsx
function hey() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  fetch(url)
    .then((res) => res.json()) // 요청해서 받은 데이터를 JSON화
    .then((data) => {
      // JSON화 한 데이터를 다시 data로 이름짓기
      let rows = data["RealtimeCityAir"]["row"];
      rows.forEach((e) => {
        // 미세먼지 데이터 리스트의 길이만큼 반복
        console.log(e);
      });
    });
}
```

<br>

- 짜잔!🎇 미세먼지 데이터 리스트의 길이만큼 반복한 것을 볼 수 있다.
  ![](https://velog.velcdn.com/images/sieunpark/post/0868cf3b-ab2c-47c5-90b1-cbdec5226b6e/image.png)

<br>

> 이번에는 구 데이터에서 미세먼지 수치(IDEX_NM),구 이름(MSRSTE_NM)을 골라내어 출력해보자

![](https://velog.velcdn.com/images/sieunpark/post/bbe7bd4b-0e7c-44b5-bd02-42332bad6b7a/image.png)

미세먼지 수치 키 값인 "IDEX_MVL", 구 이름 키 값인 "MSRSTE_NM"의 밸류를 출력하면 된다.

```jsx
function hey() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      let rows = data["RealtimeCityAir"]["row"];
      rows.forEach((e) => {
        let gu_mise = e["IDEX_NM"]; // 미세먼지 값
        let gu_name = e["MSRSTE_NM"]; // 구 이름
        console.log(gu_mise, gu_name);
      });
    });
}
```

<br>

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/c9bbeb05-22fd-49b4-b3b9-7c296c23b88d/image.png)

<br>

---

<br>

# ▶ fetch 활용

> fetch를 console로만 보는 것이 아닌, 실제로 페이지에 적용해보자!

- 먼저 fetch 활용 연습을 위해 아래의 코드를 붙여넣어보자.

```jsx
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>미세먼지 API로Fetch 연습하고 가기!</title>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>

    <style type="text/css">
      div.question-box {
        margin: 10px 0 20px 0;
      }
    </style>

    <script>
      function q1() {
        // 여기에 코드를 입력하세요
      }
    </script>
  </head>

  <body>
    <h1>Fetch 연습하자!</h1>

    <hr />

    <div class="question-box">
      <h2>1. 서울시 OpenAPI(실시간 미세먼지 상태)를 이용하기</h2>
      <p>모든 구의 미세먼지를 표기해주세요</p>
      <p>업데이트 버튼을 누를 때마다 지웠다 새로 씌여져야 합니다.</p>
      <button onclick="q1()">업데이트</button>
      <ul id="names-q1">
        <li>중구 : 82</li>
        <li>종로구 : 87</li>
        <li>용산구 : 84</li>
        <li>은평구 : 82</li>
      </ul>
    </div>
  </body>
</html>
```

<br>

- 아까 사용했던 hey() 메서드에 있는 내용을 q1() 메서드로 옮겨주자

```jsx
function q1() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      let rows = data["RealtimeCityAir"]["row"];
      rows.forEach((e) => {
        let gu_mise = e["IDEX_NM"]; // 미세먼지 값
        let gu_name = e["MSRSTE_NM"]; // 구 이름
        console.log(gu_mise, gu_name);
      });
    });
}
```

<br>

- 우리가 하고싶은 것은 ① 내용을 ②에 표기하는 것이다.
  ![](https://velog.velcdn.com/images/sieunpark/post/93d74c0e-ba0d-499f-bbcc-b016efced380/image.png)

<br>

- 따라서 저번 포스팅에서부터 계속 언급했듯 생성하는 것은 html을 만들어서 그걸 붙이는 것이기 때문에 먼저 html을 만들어야한다! (계속 복습에 복습이다. 한번 깨달으면 어렵지 않다!)

```jsx
function q1() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      let rows = data["RealtimeCityAir"]["row"];
      rows.forEach((e) => {
        let gu_mise = e["IDEX_NM"]; // 미세먼지 값
        let gu_name = e["MSRSTE_NM"]; // 구 이름

        let temp_html = `<li>${gu_mise} : ${gu_name}</li>`;
        $("#names-q1").append(temp_html);
      });
    });
}
```

<br>

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/5a5d610f-2212-4c62-bd88-f5a046e47cda/image.png)

<br>

> ⚠️ 하지만, 위 코드에는 치명적인 단점이 존재한다.

업데이트 버튼을 누를 때마다 데이터가 쌓이는 것!!!!
그래서 업데이트 버튼을 누를 때마다 지웠다 새로 쓰여지도록 코드를 수정해보자.

fetch를 하기 전에 empty를 사용하면 된다.

```jsx
      function q1() {
        let url = "http://spartacodingclub.shop/sparta_api/seoulair";

        $("#names-q1").empty();

        fetch(url)
          .then((res) => res.json())
          .then((data) => {
            let rows = data["RealtimeCityAir"]["row"];
            rows.forEach((e) => {
              let gu_mise = e["IDEX_NM"]; // 미세먼지 값
              let gu_name = e["MSRSTE_NM"]; // 구 이름

              let temp_html = `<li>${gu_mise} : ${gu_name}</li>`;
              $("#names-q1").append(temp_html);
            });
          });
      }
    </script>
```

<br>

> 반복문을 사용하여 미세먼지 수치가 좋음인 곳은 파랗게 보여줘보자.

- 우선 전체 데이터를 파랗게 보여줘보자. 그냥 class를 주면 된다.

```jsx
      .good {
        color: blue;
      }
```

```jsx
let temp_html = `<li class="good">${gu_mise} : ${gu_name}</li>`;
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/de1126c8-58f8-43bf-a1a7-b62350988b60/image.png)

<br>

- 그럼 이제 반복문을 사용하여 미세먼지 수치가 좋음인 곳은 파랗게 보여줘보자.

```jsx
function q1() {
  let url = "http://spartacodingclub.shop/sparta_api/seoulair";

  $("#names-q1").empty();

  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      let rows = data["RealtimeCityAir"]["row"];
      rows.forEach((e) => {
        let gu_mise = e["IDEX_NM"]; // 미세먼지 값
        let gu_name = e["MSRSTE_NM"]; // 구 이름

        let temp_html = ``; // 비어있는 temp_html을 먼저 생성
        if (gu_mise == "좋음") {
          //  gu_mise를 확인한 값이 "좋음"일 때, 클래스 적용
          temp_html = `<li class="good">${gu_mise} : ${gu_name}</li>`;
        } else {
          // 아니면 클래스 적용x
          temp_html = `<li>${gu_mise} : ${gu_name}</li>`;
        }

        $("#names-q1").append(temp_html);
      });
    });
}
```

<br>

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/c86e854b-4181-4010-988f-731f6946e6f5/image.png)
