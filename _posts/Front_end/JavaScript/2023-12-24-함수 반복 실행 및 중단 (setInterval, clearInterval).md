---
title: "[JS] 함수 반복 실행 및 중단(setInterval / clearInterval)"
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

> 함수를 주기적으로 반복적으로 실행하고 중단하기 위해 아래와 같은 함수를 사용한다.

<br>

# 1. setInterval

> 일정시간 마다 함수를 반복적으로 실행하는 함수이다.

- 시간은 1ms 단위이며, 1초마다 실행시키고싶으면 1000ms 를 쓰면 된다.
- handler 는 반복적으로 실행할 함수를 말한다.

```jsx
setInterval(handler, 시간);
```

<br>

- Hello!라는 문자열을 콘솔에 3초에 1번씩 실행하는 타이머를 설정한다.

```jsx
function test() {
  console.log("Hello!");
}
setInterval(test, 3000);
```

<br><br>

# 2. setInterval

> setInterval의 실행을 멈추게하는 함수이다.

```jsx
clearInterval(setInterval로 생성된 함수)
```

<br>

- 5초 후에 clearInterval() 함수를 사용하여 타이머 중단하는 함수이다.

```jsx
setTimeout(function () {
  clearInterval(intervalId);
}, 5000);
```

<br><br>

# 3. 참조

1. https://programming119.tistory.com/89
2. https://steady-dev.tistory.com/192
3. https://kyounghwan01.github.io/blog/JS/JSbasic/intervalFunction/#setinterval
