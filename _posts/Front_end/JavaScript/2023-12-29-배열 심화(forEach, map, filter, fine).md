---
title: "[JS] 배열 심화 (forEach, map, filter, fine)"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

# ▶ forEach

> - 배열의 각 요소에 대해 《 [콜백 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》를 실행한다.

- 콜백 함수란? 매개변수 자리에 함수를 넣는 것
- 즉, 배열의 각 요소에 대해 주어진 함수를 순서대로 한 번씩 실행하는 메서드이다. (배열 요소 순환 처리)

- map() 메서드와 거의 비슷하지만, return 하는 값이 없다!

<br>

> 예시

```jsx
const color = ["red", "pink"];

color.forEach(function (key) {
  console.log(`내가 좋아하는 색깔은 ${key}입니다!`);
});
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/1b9e348a-99d5-456f-9e1e-40920cfb053a/image.png)

<br>

```jsx
let number = [3, 2, 7, 4, 5];
number.forEach(function (item) {
  console.log(item);
});
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/1e0408df-1bd4-4c34-879c-e56d8eaaa4fe/image.png)

<br>

---

<br>

# ▶ map

> 반복문을 돌며 배열 안의 요소들을 <span style="color:indianred">새로운 배열로 리턴</span>한다. (매핑한다.)

- 기존에 있던 배열을 <span style="color:CornflowerBlue">가공해서</span> 새로운 배열을 생산하기 때문에, <span style="color:indianred">return값이 존재</span>한다!
- 새로운 배열을 생산하기 때문에 변수에 값을 담아줘야한다.

```jsx
let newNumbers = number.map(function (item) {
  return item * 2;
});
console.log(newNumbers);
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/039b464e-5eb6-48a1-9320-8c6d37e7f9a3/image.png)

<br>

⭐ 항상 원본 배열의 길이만큼이 retrun 된다!
➡️ return이 없으면? undefined가 출력된다.

```jsx
let newNumbers = number.map(function (item) {});
console.log(newNumbers);
```

- 즉, 리턴을 안하더라도 무조건 개수 만큼 return이 되는 것이다!
  ![](https://velog.velcdn.com/images/sieunpark/post/b0ec67fd-b632-42a8-8745-9ce35f755ce1/image.png)

<br>

---

<br>

# ▶ filter

> - 배열의 각 요소에 대해 콜백 함수를 실행하고, 그 결과가 true인 요소만 새로운 배열로 반환한다.

- 가공한 값이 들어가는 map과는 다르게 필터링할 조건에 해당되는 것만 return한다.

```jsx
let Numbers = number.filter(function (item) {
  return item === 3; // 3인 것만 뽑아낸다.
});
console.log(Numbers);
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/1bc79929-7874-41ce-b961-1e45d02da8c6/image.png)

<br>

---

<br>

# ▶ fine

> - 배열의 각 요소에 대해 콜백 함수를 실행하고, 그 결과가 true인 <span style="color:indianred">첫 번째 요소</span>를 반환한다.

- 즉, 조건에 해당되는 것만 return 하게 되지만, 첫 번째로 들어오는 것만 return한다.

```jsx
let Numbers = number.find(function (item) {
  return item > 3; // 첫 번째 요소인 7만 반환한다..
});
console.log(Numbers);
```

- 짜잔!🎇
  ![](https://velog.velcdn.com/images/sieunpark/post/f2210a71-69a1-4932-b3c6-48b19349164a/image.png)
