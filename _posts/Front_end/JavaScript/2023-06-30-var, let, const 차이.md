---
title: "[JS] var, let, const 차이"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

# ▶ var, let, const 차이

> var, let, const는 JavaScript에서 변수를 선언할 때 사용한다.

<span style="color:indianred">⚠️</span> ES6 이후부터 var은 사용하지 않는 추세이며 재할당이 필요한 경우에만 let을 사용한다.

|        구분         |     var     |     let     |    const    |
| :-----------------: | :---------: | :---------: | :---------: |
| 변수 중복 선언 허용 |      O      |      X      |      X      |
|    재할당 가능성    |      O      |      O      |      X      |
|      유효범위       | 함수 스코프 | 블록 스코프 | 블록 스코프 |

<br>

> var의 문제점

- 변수 중복 선언 허용
  - var은 let, const와 다르게 같은 스코프 내에서 중복 선언을 허용한다.
  - 아래 코드와 같이 변수가 선언되어있는 것을 모르고 의도치 않게 변수를 변경할 수 있다는 문제점이 있다.
  ```jsx
  var x = 1;
  var x = 100;
  console.log(x); // 100
  ```

<br>

---

<br>

# ▶ 스코프

## ▷ 함수 레벨 스코프

> var은 오로지 함수 코드 블록만을 지역 스코프로 인정하는 **함수 레벨 스코프**를 따른다.

```jsx
var i = 10;
if (true) {
  var i = 20;
  console.log(i); // 20
}
console.log(i); //20
```

위 코드에서 var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

<br>

## ▷ 블록 레벨 스코프

> 반면 let과 const 는 모든 코드 블록을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따른다.

```jsx
let i = 10;
if (true) {
  let i = 20;
  console.log(i); // 20
}
console.log(i); // 10
```

따라서 변수가 선언된 블록에서만 유효하다.

<br>

---

<br>

# ▶ 호이스팅과 TDZ

[호이스팅](https://velog.io/@sieunpark/%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EA%B3%BC-TDZ)에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
단, 할당 이전에 변수를 참조하면 ` undefined`를 반환한다.

> js에서 변수는 "선언 -> 초기화 -> 할당" 세 단계로 실행된다.

```jsx
// 1.선언: 변수 호이스팅에 의해 이미 value 변수 암묵적으로 실행
// 2.초기화: 변수 valeu는 undefined로 초기화
console.log(value); // undefined

// 3.할당: 변수에 값을 할당
value = 10;
console.log(value); // 10
var value;
```

이처럼 변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의해 에러를 발생시키지는 않지만 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

<br>

하지만, let과 const로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
console.log(value);
let value;
```

⚠️ let과 const도 호이스팅은 발생한다!!! 단, 발생하지 않는 것처럼 동작하는 것 뿐이다. 왜 그럴까?

> let과 const는 선언 단계와 초기화 단계가 분리되어 진행되기 때문이다!!

```jsx
// 1.선언: 변수 호이스팅에 의해 이미 value 변수 암묵적으로 실행
// (⚠️ TDZ: 초기화 이전 TDZ(일시적 사각지대)에서는 변수를 참조할 수 없다.)
console.log(value); // ReferenceError

let value; // 2.초기화: 변수 선언문에서 초기화 단계 실행
console.log(value); // undefined

value = 1; // 3.할당: 할당문에서 할당 단계가 실행된다.
console.log(value);
```

이처럼 let과 const는 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다.

> - let, const로 선언한 변수는 스코프의 선언 지점부터 초기화 시작 지점(변수 선언문)까지의 구간까지 변수를 참조할 수 없다.

- 이 구간을 일시적 사각지대 TDZ라고 부른다!

<br>

결국 let, const 키워드로 선언한 변수는 TDZ(일시적 사각지대) 때문에 호이스팅이 발생하지 않는 것처럼 보이는 것이다! 아래 코드를 보자.

```jsx
let value = 10; // 전역 변수
{
  console.log(value); // ReferenceError
  let value = 20; // 지역 변수
}
```

- 만약 let, const로 선언한 변수가 호이스팅이 발생하지 않는다면 위 코드는 전역변수 value의 값인 10을 출력해야한다.
- 하지만, 호이스팅이 발생하기 때문에 참조에러가 발생하는 것이다
  (이해가 잘 안간다...😭)

<br>

# ▶ const의 특징

let과 const 키워드는 대부분 동일하지만, 몇 가지 다른 점이 존재한다.

## ▷ 선언과 초기화

> const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

- 문법 에러 발생

```jsx
const x;
```

- 정상출력

```jsx
const x = 10;
```

<br>

## ▷ 재할당 금지

> var, let으로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const x = 10;
x = 2;
```

<br>

> const 키워드의 위와 같은 특성을 이용해 상수를 표현하기도 한다.

- 상수는 변수의 반대 개념으로 재할당이 금지된 변수를 말하는데, 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.
  - 대문자 상수는 ‘하드 코딩한’ 값의 별칭을 만들 때 사용한다.
  - 즉, 실행 전에 이미 값을 알고 있고, 코드에서 직접 그 값을 쓰는 경우에 사용한다.
    <br>
- const로 선언한 변수를 '상수(constant)'라고 부른다.

```jsx
const TAX_RATE = 0.1;
```

<br>

> 단, const 키워드로 선언된 변수에 원시 값이 아닌 객체를 할당한 경우 값을 변경할 수 있다.

왜 객체만 편애하는걸까?😯 변경 불가능한 값인 원시 값은 재할당 없이 변경할 수 없지만, 변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

```jsx
const person = {
  name: "시은",
};
person.name = "천재시은";
console.log(person); // { name: '천재시은' }
```
