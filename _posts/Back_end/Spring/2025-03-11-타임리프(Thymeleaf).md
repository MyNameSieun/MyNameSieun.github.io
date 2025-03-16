---
title: "[Spring] 타임리프(Thymeleaf)"
categories: [Spring]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

---

Thymeleaf(타임리프) 타임리프는 Spring Boot에서 HTML을 동적으로 렌더링하는 서버 사이드 템플릿 엔진이다.

이번 포스팅에선 Thymeleaf(타임리프)에 대해 자세히 알아보자.

---

<br>

# 1. 타임리프 개요

## 1.1 타임리프 특징

> ① 순수 HTML을 유지하는 네츄럴 템플릿(Natural Templates)

타임리프 문법이 들어간 HTML 파일을 웹 브라우저에서 그대로 열어도 정상적으로 보인다.

JSP와 달리 서버가 없어도 HTML 파일이 깨지지 않는다.

<br>

> ② 서버에서 데이터와 함께 렌더링 가능

컨트롤러에서 전달한 데이터를 활용하여 동적으로 HTML을 생성할 수 있다.

```html
<p th:text="${message}">기본 값</p>
```

<br>

> ③ 다양한 HTML 속성을 변경 가능

기존 HTML 속성을 유지하면서 타임리프를 활용할 수 있다.

```html
<a href="link.html" th:href="@{/new-link}">링크</a>
```

서버가 렌더링하면 `th:href` 값이 `href` 값으로 대체된다.

<br>

> 타임리프는 순수 HTML 파일을 웹 브라우저에서 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.

- JSP를 생각해보면, JSP 파일은 웹 브라우저에서 그냥 열면 JSP 소스코드와 HTML이 뒤죽박죽 되어서 정상적인 확인이 불가능하다.
- 오직 서버를 통해서 JSP를 열어야 한다.
- 이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿(natural templates)이라 한다.

<br>

## 1.2 타임리프 템플릿 엔진 동작 원리

[http://localhost:8080/hello](http://localhost:8080/hello) 을 요청했다고 가정

![](/assets/images/2025/2025-03-14-19-46-02.png)

> ① 브라우저에서 요청

웹 브라우저가 [http://localhost:8080/hello](http://localhost:8080/hello)로 요청을 전송한다.

<br>

> ② 스프링 부트의 내장 톰캣(Web 서버)이 요청을 처리

내장된 톰캣 서버가 요청을 받아 Spring MVC로 전달한다.

<br>

> ③ HelloController의 hello 메서드 실행

`@GetMapping("hello")`에 매핑된 hello 메서드가 호출된다.

메서드는 Model 객체에 데이터를 추가한다.

```java
@Controller
public class HelloController {

    // hello 메서드 호출
    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data","hello!!!");
        return "hello";
    }
}
```

<br>

> ④ **뷰 리졸버(ViewResolver)** 가 HTML 템플릿 찾기

`hello` 메서드는 `return "hello";`를 통해 뷰 이름을 반환한다.

스프링의 뷰 리졸버(ViewResolver)가 템플릿 파일 경로를 구성한다.

`resources/template/` + {ViewName} + `.html`

```bash
resources/templates/hello.html
```

위 경로의 HTML 파일이 렌더링 대상이다.

<br>

> ⑤ Thymeleaf가 템플릿 처리

`hello.html` 파일을 로드하고, HTML 템플릿 내부의 Thymeleaf 문법(th:text)에 따라 서버에서 데이터를 동적으로 주입한다.

```html
<p th:text="|안녕하세요 ${data}|">안녕하세요. 손님</p>
```

`${data}`는 컨트롤러에서 전달한 `model.addAttribute`의 값을 참조한다.

최종 결과는 다음과 같이 렌더링된다.

```html
<p>안녕하세요. hello!!!</p>
```

<br>

> ⑥ 렌더링된 HTML 반환

Thymeleaf가 데이터가 포함된 최종 HTML을 생성하여 클라이언트(브라우저)에 전달한다.

<br>

> 참고

- spring-boot-devtools 라이브러리를 추가하면, html 파일을 컴파일만 해주면 서버 재시작없이 View 파일 변경이 가능하다.
- IntelliJ 컴파일 방법: 메뉴 build -> Recompile

<br><br>

# 2. 타임리프 사용 예제

자세한 내용은 [[Spring MVC 웹페이지 만들기]](https://mynamesieun.github.io/spring/Spring-MVC-%EC%9B%B9%ED%8E%98%EC%9D%B4%EC%A7%80-%EB%A7%8C%EB%93%A4%EA%B8%B0/) 포스팅을 참고하자

## 2.1 컨트롤러

```java
@Controller
@RequestMapping("/basic/items")
@RequiredArgsConstructor
public class BasicItemController {
    private final ItemRepository itemRepository;

    @GetMapping
    public String item(Model model){
        List<Item> items = itemRepository.findAll();
        model.addAttribute("items", items);
        return "basic/item";
    }

    @PostConstruct
    public void init(){
        itemRepository.save(new Item("감자",1000,2));
        itemRepository.save(new Item("고구마",3000,2));
    }
}
```

<br>

## 2.2 타임리프

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="utf-8" />
    <link
      href="../css/bootstrap.min.css"
      th:href="@{/css/bootstrap.min.css}"
      rel="stylesheet"
    />
  </head>
  <body>
    <div class="container" style="max-width: 600px">
      <div class="py-5 text-center">
        <h2>상품 목록</h2>
      </div>
      <div class="row">
        <div class="col">
          <button
            class="btn btn-primary float-end"
            onclick="location.href='addForm.html'"
            th:onclick="|location.href='@{/basic/items/add}'|"
            type="button"
          >
            상품 등록
          </button>
        </div>
      </div>
      <hr class="my-4" />
      <div>
        <table class="table">
          <thead>
            <tr>
              <th>ID</th>
              <th>상품명</th>
              <th>가격</th>
              <th>수량</th>
            </tr>
          </thead>
          <tbody>
            <tr th:each="item : ${items}">
              <td>
                <a
                  href="item.html"
                  th:href="@{/basic/items/{itemId} (itemId=${item.id})}"
                  th:text="${item.id}"
                  >회원id</a
                >
              </td>
              <td>
                <a
                  href="item.html"
                  th:href="@{|/basic/items/${item.id}|}"
                  th:text="${item.itemName}"
                  >상품명</a
                >
              </td>
              <td th:text="${item.price}">10000</td>
              <td th:text="${item.quantity}">10</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

<br><br>

# 3. 타임리프의 주요 문법

## 3.1 타임리프 사용 선언

```html
<html xmlns:th="http://www.thymeleaf.org"></html>
```

<br>

## 3.2 변수 표현식: ${...}

> 모델 데이터를 가져와서 출력한다.

```html
<p th:text="${user.name}">홍길동</p>
```

<br>

## 3.3 URL 링크 표현식: @{...}

> URL을 동적으로 생성할 때 사용한다.

타임리프는 URL 링크를 사용하는 경우 `@{...}` 를 사용한다. 이것을 URL 링크 표현식이라 한다.

```html
<a th:href="@{/items/{id}(id=${item.id})}">상품 상세</a>
```

- URL 링크 표현식을 사용하면 경로를 템플릿처럼 편리하게 사용할 수 있다.
- 경로 변수( `{itemId}` ) 뿐만 아니라 쿼리 파라미터도 생성한다.
  ```html
  th:href="@{/basic/items/{itemId}(itemId=${item.id}, query='test')}"
  ```
  - 생성 링크: [http://localhost:8080/basic/items/1?query=test](http://localhost:8080/basic/items/1?query=test)

<br>

## 3.4 속성 변경: th:xxx

> HTML 속성을 변경할 때 사용한다.

```html
<img th:src="@{/images/logo.png}" />
```

- `th:xxx` 가 붙은 부분은 서버사이드에서 렌더링 되고, 기존 것을 대체한다.
- `th:xxx` 이 없으면 기존 html의 `xxx` 속성이 그대로 사용된다.
- HTML을 파일로 직접 열었을 때, `th:xxx` 가 있어도 웹 브라우저는 `th:` 속성을 알지 못하므로 무시한다.
- 따라서 HTML을 파일 보기를 유지하면서 템플릿 기능도 할 수 있다.

<br>

## 3.5 반복문: th:each

> 리스트를 반복 출력할 때 사용한다.

```html
<tr th:each="item : ${items}">
  <td th:text="${item.name}"></td>
</tr>
```

<br>

## 3.6 조건문: th:if, th:unless

> 조건에 따라 특정 요소를 표시하거나 숨길 수 있다.

```html
<p th:if="${user.isAdmin}">관리자 계정</p>
```

<br>

## 3.7 리터럴 대체: |...|

> 문자열을 조합할 때 사용한다.

```html
<p th:text="|안녕하세요, ${user.name}님!|">기본 값</p>
```

`${user.name}` 값이 홍길동이라면, 안녕하세요, 홍길동님!으로 변환된다.

<br>

- 타임리프에서 문자와 표현식 등은 분리되어 있기 때문에 더해서 사용해야 한다.
  ```html
  <span
    th:text="'Welcome to our application, ' + ${[user.name](http://user.name/)} + '!'"
  ></span>
  ```
- 다음과 같이 리터럴 대체 문법을 사용하면, 더하기 없이 편리하게 사용할 수 있다.

  ```html
  <span
    th:text="|Welcome to our application, ${[user.name](http://user.name/)}!|"
  ></span>
  ```

- 결과를 다음과 같이 만들어야 하는데

  ```html
  location.href='/basic/items/add'
  ```

- 그냥 사용하면 문자와 표현식을 각각 따로 더해서 사용해야 하므로 다음과 같이 복잡해진다.

  ```html
  th:onclick="'location.href=' + '\'' + @{/basic/items/add} + '\''"
  ```

- 리터럴 대체 문법을 사용하면 다음과 같이 편리하게 사용할 수 있다.

  ```html
  th:onclick="|location.href='@{/basic/items/add}'|"
  ```

<br>
