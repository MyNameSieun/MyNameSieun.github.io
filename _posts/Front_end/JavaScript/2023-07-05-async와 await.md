---
title: "[JS] async와 await"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

[[프로미스]](https://mynamesieun.github.io/javascript/%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4/) 에 이어 작성하는 글입니다.
{: .notice--danger}

<br>

# 1. async ~ await 등장 배경

## 1.1 callback hell

"콜백 지옥"은 콜백 함수가 중첩되어 복잡하고 가독성이 떨어지는 코드 패턴을 말한다.

then과 catch를 사용하는 비동기 프로그래밍에서 이러한 상황이 발생하기 쉽다.

```js
// case1
getData(function(a) {
 getMoreData(a, function(b) {
   getMoreData(b, function(c) {
     getMoreData(c, function(d) {
       getMoreData(d, function(e) {
         // 콜백 내부에서 또 다른 콜백을 호출
       });
     });
   });
 });
});

// case2
fetch('https://jsonplaceholder.typicode.com/posts/1')
 .then(response => {
		// response의 id로 다른 작업 하기
		fetch(`http://testServer.com/${response.id}`)
			.then(response2 => {
				fetch(`http://anotherServer.com/${response2.id}`)
					.then(~~~~~~~~)
					.catch(error => console.error('Error:', error));;
			})
			.catch(error => console.error('Error:', error));
	})
 .catch(error => console.error('Error:', error));
```

<br>

## 1.2 프로미스 체이닝

![](https://velog.velcdn.com/images/sieunpark/post/03e5d6e6-c0e9-4389-baad-bb20105eeffc/image.png)

프로미스를 사용하면 프로미스 체이닝을 사용한다.

따라서 작업이 많아지면 체인의 길이가 길어지고 코드가 복잡해져 버그를 찾거나 수정하기 어려울 수 있다.

⚠️ 프로미스를 쓰는 것이 효율적인 상황도 있다.

<br>

async ~ await를 사용하면 이러한 문제를 해결할 수 있다!

<br><br>

# 2. async/await 개요

## 2.1 async/await 개념

async와 await는 프로미스를 조금 더 간결하게 만들어주고 콜백 함수와 프로미스 체인의 단점을 보완해준다.

또한 동기적으로 코드를 작성하듯(평소와 같이 코드를 작성하듯) 쓸 수 있어 간편하다.<br>
비동기적 표현을 동기적으로 바꿨는데도 정말 동기적으로 작성한 것처럼 보이는 것이다!

<br>

## 2.2 async 사용하기

```js
// 정말 동기적으로 작성한 것처럼 보인다!
async function processData() {
  try {
    // await라는 키워드를 이용해서 수행중인 작업이 끝나야지만 아래 작업이 실행되게 한다.
    // 즉, 해당 키워드가 사용된 위치에서 비동기 작업이 완료될 때까지 코드의 실행을 일시 중단하는 것이다.
    let a = await getData();
    let b = await getMoreData(a);
    let c = await getMoreData(b);
    let d = await getMoreData(c);
    let e = await getMoreData(d);
    // 여기서 e를 사용하여 추가 작업을 수행할 수 있다.
  } catch (error) {
    console.error("Error:", error);
  }
}

processData();
```

async를 어디 넣어야할지 모르겠으면 await만 쓴다. 즉 언제 기다려야 하는지를 먼저 정의하자.<br>
그 후, 가장 가까운 함수에 async를 붙히면 된다.

<br>

> try ~ catch를 사용해보자

try ~ catch문은 예외를 처리하기 위한 구문이다.<br>
"try" 블록 안에서 예외가 발생하면 "catch" 블록이 실행되어 예외를 처리한다.

try(시도하다), catch(오류를) 잡아낸다.<br>
즉, 시도할 내용을 try 블럭에 적고 예외 처리를 catch 블록에 적으면 되는 것이다.

```js
async function fetchPosts() {
  try {
    const response = await fetch(
      "https://jsonplaceholder.typicode.com/posts/1"
    );
    const data = await response.json(); // response의 id 사용을 위해 json으로 변환
    const response2 = await fetch(`http://testServer.com/${data.id}`);
    const data2 = await response2.json();
    const finalResponse = await fetch(`http://anotherServer.com/${data2.id}`);
    const finalData = await finalResponse.json();
    // finalData를 사용하여 추가 작업을 수행할 수 있다.
  } catch (error) {
    console.error("Error:", error);
  }
}

fetchPosts();
```

<br>

> 프로미스를 이용해 유저의 데이터를 비동기적으로 작성해보자

```jsx
function fetchUser() {
  return new Promise((resolve, reject) => {
    resolve("sieun");
  });
}
const user = fetchUser();
user.then(console.log);
console.log(user);
```

<br>

> async를 사용하여 프로미스를 간편하게 비동기적으로 작성해보자

```jsx
async function fetchUser() {
  return "sieun";
}
const user = fetchUser();
console.log(user);
```

함수 앞에 async를 키워드를 붙여주면 번거롭게 프로미스를 쓰지 않아도 자동적으로 함수 안에 있는 코드 블록들이 프로미스로 변환된다.

<br>

## 2.3 Promise + async/await

- 비동기 작업을 수행코자 하는 함수 앞에 async 함수 내부에서 실질적인 비동기 작업이 필요한 위치마다 await를 붙여주면 된다.
- Promise ~ then과 동일한 효과를 얻을 수 있다.

```jsx
// addCoffee 함수에서 호출할 함수, "addCoffee"를 선언
// Promise를 반환
var addCoffee = function (name) {
  return new Promise(function (resolve) {
    setTimeout(function () {
      resolve(name);
    }, 500);
  });
};

/// ⭐
var coffeeMaker = async function () {
  // var coffeeMaker = async () = {
  var coffeeList = "";
  var _addCoffee = async function (name) {
    coffeeList += (coffeeList ? ", " : "") + (await addCoffee(name));
  };

  // Promise를 반환하는 함수인 경우, awite를 만나면 무조건 끝날 때 까지 기다린다.
  // _addCoffee("에소프레소")이 로직이 실행되는데 100초가 걸리면
  await _addCoffee("에스프레소");

  // console.log는 100초 뒤 실행
  console.log(coffeeList);
  await _addCoffee("아메리카노");
  console.log(coffeeList);
  await _addCoffee("카페모카");
  console.log(coffeeList);
  await _addCoffee("카페라떼");
  console.log(coffeeList);
};
coffeeMaker();
```

<br><br>

# 3. 문제📝

아래의 코드를 async/await 로 리팩토링을 해주세요.

```jsx
class HttpError extends Error {
  constructor(response) {
    super(`${response.status} for ${response.url}`);
    this.name = "HttpError";
    this.response = response;
  }
}

function loadJson(url) {
  return fetch(url).then((response) => {
    if (response.status == 200) {
      return response.json();
    } else {
      throw new HttpError(response);
    }
  });
}

function narutoIsNotOtaku() {
  let title = prompt("애니메이션 제목을 입력하세요.", "naruto");

  return loadJson(
    `https://animechan.vercel.app/api/random/anime?title=${title}`
  )
    .then((res) => {
      alert(`${res.character}: ${res.quote}.`);
      return res;
    })
    .catch((err) => {
      if (err instanceof HttpError && err.response.status == 404) {
        alert(
          "일치하는 애니메이션이 없습니다. 일반인이시면 naruto, onepiece 정도나 입력해주세요!"
        );
        return narutoIsNotOtaku();
      } else {
        throw err;
      }
    });
}

narutoIsNotOtaku();
```

> 코드 이해하기

1. 먼저 콘솔에 위의 코드를 입력하고 엔터를 입력해주세요
2. 해당 코드는 애니메이션 제목을 입력하면, 해당 애니메이션의 캐릭터와 명대사를 출력해주는 코드입니다.
3. 나루토를 입력하면..<br><br>
   ![](https://velog.velcdn.com/images/sieunpark/post/1f69d2a2-4161-400b-a339-2473d32e5b14/image.png)<br><br>
4. 다음과 같이 결과가 나옵니다.<br><br>
   ![](https://velog.velcdn.com/images/sieunpark/post/bc7bfc30-88e4-4ced-ab3d-c413d85e052d/image.png)
5. 먼저 **fetch() 함수는** 지금은 browser에서 네트워크 통신을 할 수 있도록 해두는 함수라고만 이해하시면 좋을 것 같습니다. “네트워크 통신”이므로 결과로 프로미스를 반환하는 대표적인 함수 입니다.
6. 그걸 감싼 loadJson() 함수는 아주 간단하게, url을 입력받아 fetch 함수를 호출해주고, 그 통신이 성공했을 때(statusCode 200), 결과를 반환해주는 함수입니다.
7. 만약 통신이 실패하는 경우 위에 작성한(지금은 이해 못하셔도 상관없습니다) 에러 객체를 반환해줍니다.
8. 그리고 아래의 코드에서, loadJson() 함수의 결과를 받아, 결과값을 화면에 띄워주는 일을 하고 있습니다.

```jsx
function narutoIsNotOtaku() {
  let title = prompt("애니메이션 제목을 입력하세요.", "naruto");

  return loadJson(
    `https://animechan.vercel.app/api/random/anime?title=${title}`
  )
    .then((res) => {
      alert(`${res.character}: ${res.quote}.`);
      return res;
    })
    .catch((err) => {
      if (err instanceof HttpError && err.response.status == 404) {
        alert(
          "일치하는 애니메이션이 없습니다. 일반인이시면 naruto, onepiece 정도나 입력해주세요!"
        );
        return narutoIsNotOtaku();
      } else {
        throw err;
      }
    });
}
```

<br>

이제 위의 코드를 async/await를 이용하여 리팩토링 후 제출해주세요!

- class HttpError extends Error 쪽은 수정하지 않아도 됩니다.
- 꼭 결과를 확인하고 제출해주세요

<br>

## 전체 코드

```jsx
class HttpError extends Error {
  constructor(response) {
    super(`${response.status} for ${response.url}`);
    this.name = "HttpError";
    this.response = response;
  }
}

async function loadJson(url) {
  // promise then 부분
  let response = await fetch(url);
  if (response.status == 200) {
    return response.json();
  } else {
    throw new HttpError(response);
  }
}

async function narutoIsNotOtaku() {
  let title;
  while (true) {
    title = prompt("애니메이션 제목을 입력하세요.", "naruto");
    // promise 체이닝 catch 부분 -> try catch문 사용해서 동일 로직 시행 가능
    try {
      res = await loadJson(
        `https://animechan.vercel.app/api/random/anime?title=${title}`
      );
      break;
    } catch (err) {
      if (err instanceof HttpError && err.response.status == 404) {
        alert(
          "일치하는 애니메이션이 없습니다. 일반인이시면 naruto, onepiece정도나 입력해주세요."
        );
      } else {
        throw err;
      }
    }
  }

  alert(`${res.character}: ${res.quote}.`);
  return res;
}

narutoIsNotOtaku();
```

<br><br>

# 4. 참조

- https://www.youtube.com/watch?v=aoQSOZfz3vQ&t=246s

<br>
