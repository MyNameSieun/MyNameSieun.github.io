---
title: "[JS] use strict"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

> use strict (엄격 모드)란?

- 암묵적인 "느슨한 모드(sloppy mode)"를 해제하고,
  명시적인 "엄격 모드(Strict Mode)"를 사용하는 방법을 말한다.
- use strict는 반드시 <span style="color:indianred">스크립트 최상단</span>에 위치시켜야한다. (취소할 수 없다.)

<br>

➡️ a에 데이터 타입을 주지 않았는데 코드가 정상적으로 실행이 되었다. 이는 js가 문법 체크에 약하기 때문!

```jsx
a = 3;
console.log(a); // 3
```

<br>

➡️ 엄격 모드를 사용해볼까? 에러가 발생한다!

```jsx
"use strict";
a = 3;
console.log(a); // error: num is not defined
```

<br>

<br>

> 엄격 모드의 장점

- 흔히 발생하는 코딩 실수를 잡아내서 예외를 발생시킨다.
- 상대적으로 안전하지 않은 액션이 발생하는 것을 방지한다.
- 정확하게 고려되지 않은 기능들을 비활성화시킨다.

<br>

> 엄격 모드가 불필요한 경우

- 클래스, 모듈의 경우 엄격모드가 기본값이므로, 별도로 지정할 필요가 없다.

```jsx
// 클래스

class SampleClass {
  ...
}
```

```jsx
<!-- 모듈 -->

<script type=”module” src="index.js" >
```

<br>

---

<br>

# 📎참조

- https://ko.javascript.info/strict-mode
- https://mong-blog.tistory.com/entry/JS-Strict-mode%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C
- https://ithub.tistory.com/162
