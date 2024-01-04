---
title: "async와 await"
categories: [JavaScript]
tag: [JavaScript]
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

《 [프로미스](https://velog.io/@sieunpark/JS-%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4) 》에 이어 작성하는 글입니다.

---

# ▶ async 와 await

![](https://velog.velcdn.com/images/sieunpark/post/03e5d6e6-c0e9-4389-baad-bb20105eeffc/image.png)

프로미스를 사용하면 프로미스 체이닝을 사용한다.

따라서 작업이 많아지면 체인의 길이가 길어지고 코드가 복잡해져 버그를 찾거나 수정하기 어려울 수 있다.

> async 와 await는 [프로미스](https://velog.io/@sieunpark/JS-%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4)를 조금 더 간결하게 만들어준다.
> 또한 동기적으로 코드를 작성하듯(평소와 같이 코드를 작성하듯) 쓸 수 있어 간편하다.

⚠️ 그렇다고해서 무조건 프로미스가 나쁜 건 아니다! 프로미스를 쓰는 것이 효율적인 상황도 있다.

<br>

---

<br>

# ▶ async 사용하기

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

---

<br>

# ▶ await 사용하기

> await이라는 키워드는 async 가 붙은 함수 안에서만 쓸 수 있다. 아래 예제를 살펴보자

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getCherry() {
  await delay(3000);
  return "🍒";
}
```

- delay()는 프로미스를 리턴하는데 정해진 ms가 지나면 resolve를 호출하는 프로미스를 리턴한다.

- 따라서 아래 함수에서는 3초가 지나면 체리를 호출하는 프로미스가 전달이 된다.

- 여기서 await이라는 키워드를 쓰게 되면 딜레이가 끝날 때까지 기다려 준다.

<br>

> await를 쓰지 않고 프로미스로 구현해보자

```jsx
function getCherry() {
  return delay(3000).then(() => "🍒");
}
```

위와 같이 체이닝을 사용하는 것 보다 await를 사용하여 동기적인 코드를 사용하는 것처럼 만들게 되면 더 쉽게 이해할 수 있게 되는 것이다!

<br>

> 모든 과일을 한 번에 출력하려면 어떻게 해야할까? 프로미스로 구현해보자.

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getCherry() {
  await delay(3000);
  return "🍒";
}
async function getBanana() {
  await delay(3000);
  return "🍌";
}

// 프로미스 체이닝 이용
function pickFruits() {
  return getCherry().then((cherry) => {
    return getBanana().then((banana) => `${cherry} + ${banana}`);
  });
}

pickFruits().then(console.log);
```

- then 메서드를 이용하여 프로미스 체이닝을 구성한다.
- pickFruits 함수는 getCherry 함수를 호출하고, 그 결과를 이용해 getBanana 함수를 호출한다.
- getCherry 함수의 프로미스가 해결되면, then 메서드 내부의 콜백 함수가 실행된다. 해당 콜백 함수는 getBanana 함수를 호출하고, 그 결과인 바나나 값을 받아온다.

<br>

🤯어지럽다.. 프로미스도 중첩적으로 체이닝을 하게되면 콜백 지옥과 비슷한 문제점들이 발생한다.

<br>

> async를 통해 위 문제를 해결해보자!

```jsx
async function pickFruits() {
  const cherry = await getCherry();
  const banana = await getBanana();
  return `${apple} + ${banana}`;
}
```

체리와 바나나를 await를 통해 받아와 리턴하였다. 간결하다!

![](https://velog.velcdn.com/images/sieunpark/post/9344fbf9-812b-4913-a5ea-d006aa34ea1f/image.png)

<br>

> 에러가 발생했다면 어떻게 처리하면 좋을까?

`throw 'error';` 을 사용하면 에러가 발생하게 되는데, 프로미스에서 에러 핸들링을 했던 것처럼 try-catch를 사용하여 에러 핸들링을 해줄 수 있다.

- try 블록 내에는 예외가 발생할 수 있는 코드를 작성하고 catch 블록은 예외가 발생할 경우 어떻게 처리할것인지 작성한다.
- catch 블록은 선택적으로 사용할 수 있다.

```jsx
try {
  const cherry = await agetCherry();
  const banana = await getBanana();
} catch (error) {
  console.log(error);
}
```

<br>

> await 병렬처리

```jsx
function delay(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function getCherry() {
  await delay(3000);
  return "🍒";
}
async function getBanana() {
  await delay(3000);
  throw "error";
  return "🍌";
}

async function pickFruits() {
  const cherry = await getCherry();
  const banana = await getBanana();
  return `${cherry} + ${banana}`;
}

pickFruits().then(console.log);
```

> 위 코드는 한 가지 문제점이 있다.

await를 사용했기 때문에 체리를 호출할 때 3초를 기다려야하고 바나나를 호출할 때 3초를 기다려야한다.

<br>

> 병렬처리를 통해 문제를 해결해보자!

프로미스를 만들면 프로미스안에 들어있는 코드 블록이 즉시 실행된다. 이 특징을 이용해 병렬 처리를 한 것이다.

```jsx
async function pickFruits() {
  const cherryPromise = getCherry();
  const bananaPromise = getBanana();

  const cherry = await cherryPromise();
  const banana = await bananaPromise();

  return `${cherry} + ${banana}`;
}

pickFruits().then(console.log);
```

<br>

병렬적으로 처리하기 이해선 위와 같이 더럽게 코드를 작성하지 않는다고 한다...

> 쉽게 병렬 처리를 할 수 있는 Promise APIs를 알아보자

`Promise.all` API를 사용하여 프로미스 배열을 전달하게되면 모든 프로미스들이 병렬적으로 다 받을 때까지 모아준다.

```jsx
function pickAllFruits() {
  return Promise.all([getCherry(), getBanana()]).then((fruits) =>
    fruits.join("+")
  );
}
pickAllFruits().then(console.log);
```

![](https://velog.velcdn.com/images/sieunpark/post/40e94688-7b2d-4ffe-99c0-bdf54d8a110b/image.png)

(API를 활용한 병렬 처리는 이해가 안되서 나중에 다시 공부하도록 하자)

<br>

---

<br>

# ▶ Promise + Async/await

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

<br>

---

<br>

# 📝 문제

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
3. 나루토를 입력하면..
   ![](https://velog.velcdn.com/images/sieunpark/post/1f69d2a2-4161-400b-a339-2473d32e5b14/image.png)
4. 다음과 같이 결과가 나옵니다.
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

<br>

---

<br>

# 📎참조

https://www.youtube.com/watch?v=aoQSOZfz3vQ&t=246s
