---
title: "[CSS] Media Query"
categories: [CSS]
tag: [CSS]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ 미디어 쿼리

## ▷ 미디어 쿼리 소개

- 반응형 디자인이란? 화면의 크기에 따라 웹페이지의 각 요소들이 반응하여 최적화 된 모양으로 동작하는 것을 말한다.
- 반응형 디자인을 CSS로 구현하기 위한 핵심적인 개념이 미디어 쿼리이다.
- 미디어 쿼리는 여러가지 형태의 화면이 존재하는 세상에서 굉장히 중요한 역할을 한다.

  <br>

> 아래의 코드를 기본형으로 하여 `<div>`태그를 화면의 크기에 따라 보이거나 안보이게 해보자.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <style>
      div {
        border: 10px solid green;
        font-size: 69px;
      }
    </style>
  </head>
  <body>
    <div>Responsive</div>
  </body>
</html>
```

![](https://velog.velcdn.com/images/sieunpark/post/3f09eb29-ecd2-4c9b-8e57-cba06fa256f6/image.png)

1. 현재 웹페이지 화면의 크기를 알아야 한다.

- 개발자 도구를 띄어 현재 화면의 크기를 확인할 수 있다.
  ![](https://velog.velcdn.com/images/sieunpark/post/61a2e257-a4a7-4d79-b253-de95a7821c7a/image.png)

<br>2. 다음과 같은 코드를 작성한다.

- 스크린의 크기가 800px보다 크다면 div태그의 디스플레이를 none으로 한다.
- 이해를 돕기 위해 다음과 같은 코드를 작성해주었다. (문법에 맞지 않는 설명만을 위한 코드이다)

```css
 screen width > 800px
```

- 위 코드를 미디어쿼리 코드로 바꿔보자
- 아래 코드는 화면의 크기가 최소 800px 보다 크다면 중괄호 안에 있는 <`div`> 태그의 효과인 none이 동작할 수 있도록 하는 코드이다.

```css
@media (min-width: 800px) {
  div {
    display: none;
  }
}
```

- 짜잔!
  ![](https://velog.velcdn.com/images/sieunpark/post/1937504e-4881-4231-84e9-730594f5d406/image.png)

전체 코드는 다음과 같다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title></title>
    <style>
      div {
        border: 10px solid green;
        font-size: 69px;
      }
      @media (max-width: 800px) {
        div {
          display: none;
        }
      }
    </style>
  </head>
  <body>
    <div>Responsive</div>
  </body>
</html>
```

 <br>

## ▷ 미디어 쿼리 사용하기

> 800px을 기준으로 800px보다 클 때와 작을 때의 디자인을 미디어 쿼리를 통해 변경해보자

- 현재 디스플레이는 그리드로 나타나있는 상태이다.![](https://velog.velcdn.com/images/sieunpark/post/97d926fb-3f78-4b07-b254-d7fc286f52f6/image.png)
- 다음과 같은 코드를 추가 하여 디스플레이를 블록으로 바꿔보자

```css
  /* screen width < 800px */
  @media(max-width:800px){
    #grid{
      display: block;
    }
```

- 짜잔!
  ![](https://velog.velcdn.com/images/sieunpark/post/99866959-e456-45bd-aca3-22071a614bf6/image.png)

 <br>
                         
>다음과 같은 코드를 추가하여 디스플레이가 800px보다 작을 때 ```<ol>``` 태그 옆의 세로선을 사라지게 해보자
  
  
```css
  @media(max-width:800px){
    #grid{
      display: block;
    }
    ol{
      border-right: none;
    }
  }
```
- 짜잔!
![](https://velog.velcdn.com/images/sieunpark/post/4f3b039d-2ef0-40b8-a2ba-b93a8b81203b/image.png)

  <br>
  
>다음과 같은 코드를 추가하여 디스플레이가 800px보다 작을 때 ```<h`>``` 태그 아래의 가로선을 사라지게 해보자
  
```css
  @media(max-width:800px){
    #grid{
      display: block;
    }
    ol{
      border-right: none;
    }
    h1{
      border-bottom: none;
    }
  }
```
- 완성!
![](https://velog.velcdn.com/images/sieunpark/post/211df2ea-0b15-4cb5-a217-ab97a4dd3266/image.png)
