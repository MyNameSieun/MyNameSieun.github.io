---
title: "[JS] Intl API로 Date 객체 쉽게 다루기"
categories: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 0. Intl API 개요

- Internationalization API는 자바스크립트에서 제공하는 국제화(Internationalization)와 지역화(Localization)를 지원하기 위한 API이다.
- 이 API는 언어 및 지역 설정에 따라 날짜와 시간을 형식화할 수 있는 기능을 제공하고, 다국적 사용자들에게 적합한 형식의 날짜, 시간, 숫자, 통화 등을 제공할 수 있다.
- 또한, JavaScript의 날짜, 숫자, 리스트 포맷팅을 보다 정교하게 제어할 수 있게 해준다. 이전의 Date 객체보다 더 많은 유연성을 제공하며, 국제화 및 지역화를 쉽게 처리할 수 있다.

| 객체 이름                 | 설명                                                                                                                                    |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `Intl.DateTimeFormat`     | - 날짜와 시간을 사용자의 지역에 맞는 형식으로 변환 <br>- ex) 한국에서의 현재 날짜와 시간을 한국식 형식으로 표시                         |
| `Intl.NumberFormat`       | - 숫자를 사용자의 지역에 맞는 형식으로 변환<br> - ex) 천 단위 구분 기호, 소수점 구분 기호, 소수점 자릿수 등을 사용자의 지역에 맞게 설정 |
| `Intl.ListFormat`         | - 배열의 요소를 사용자의 언어에 맞는 목록 형식으로 포맷팅 <br> - ex) "Apple, Banana, or Cherry"와 같이 포맷팅                           |
| `Intl.RelativeTimeFormat` | - 상대적인 시간 표현을 사용자의 지역에 맞는 형식으로 제공 <br> - "in 3 days"와 같이 상대적인 시간을 표현                                |
| `Intl.PluralRules`        | 숫자의 복수형을 처리하는 규칙을 제공<br> - ex) 사용자의 언어에 따라 복수형 처리를 할 때 유용                                            |

<br>

# 1. Intl.DateTimeFormat

> Intl.DateTimeFormat 객체는 날짜와 시간을 지역화된 형식으로 표시하는 데 사용된다. 예를 들어, 특정 언어와 지역에 따라 날짜와 시간을 포맷팅하고, 원하는 형식으로 조정할 수 있다. [[공식문서↗️]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat)

```js
const date = new Date();
const options = {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
};

console.log(new Intl.DateTimeFormat("en-US", options).format(date)); // "Thursday, July 12, 2024"
console.log(new Intl.DateTimeFormat("ko-KR", options).format(date)); // "2024년 7월 12일 목요일"
```

<br>

# 2. Intl.NumberFormat

> Intl.NumberFormat 객체는 숫자를 지역화된 형식으로 표시하는 데 사용된다. 숫자의 통화, 소수점 표기법, 그룹화 등을 사용자의 언어와 지역에 맞게 형식화할 수 있다

```js
const number = 123456.789;

console.log(new Intl.NumberFormat("en-US").format(number)); // "123,456.789"
console.log(new Intl.NumberFormat("de-DE").format(number)); // "123.456,789"
```

<br>

# 3. Intl.ListFormat

> Intl.ListFormat 객체는 리스트 형식의 텍스트를 지역화된 방식으로 포맷팅하는 데 사용된다. 다양한 리스트 요소 사이에 적절한 구분자를 넣어준다.

```js
const fruits = ["apple", "banana", "cherry"];

console.log(
  new Intl.ListFormat("en", { style: "long", type: "conjunction" }).format(
    fruits
  )
); // "apple, banana, and cherry"
console.log(
  new Intl.ListFormat("de", { style: "long", type: "disjunction" }).format(
    fruits
  )
); // "apple, banana oder cherry"
```

<br>

# 4. Intl.RelativeTimeFormat

> Intl.RelativeTimeFormat 객체는 상대적 시간(예: "2 days ago")을 지역화된 방식으로 표시하는 데 사용된다.

```js
const rtf = new Intl.RelativeTimeFormat("en", { numeric: "auto" });

console.log(rtf.format(-1, "day")); // "yesterday"
console.log(rtf.format(2, "day")); // "in 2 days"
```

<br>
