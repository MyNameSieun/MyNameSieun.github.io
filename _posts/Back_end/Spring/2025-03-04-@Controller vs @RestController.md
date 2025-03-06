---
title: "[Spring] @Controller vs @RestController"
categories: [Spring]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

Spring Framework에서는 클라이언트의 요청을 처리하고 응답을 반환하기 위해 @Controller와 @RestController 어노테이션을 제공한다.

이 두 어노테이션의 차이점과 사용 방법에 대해 알아보자.

<br>

# 1. @Controller

> `@Controller`는 주로 **뷰(View)를 반환**할 때 사용되는 어노테이션이다.

JSP, Thymeleaf, Freemarker 등의 뷰 템플릿을 이용해 HTML을 렌더링할 때 사용된다.

<br>

> 특징

- 메서드에서 return "뷰 이름"을 반환하면 해당 이름의 HTML 파일을 렌더링한다.
- Model 객체를 이용해 데이터를 뷰에 전달할 수 있다.
- 주로 서버 사이드 렌더링(SSR) 방식에 사용된다.

```java
@Controller
public class PageController {

    @GetMapping("/welcome")
    public String welcome(Model model) {
        model.addAttribute("name", "시은");
        return "welcome"; // welcome.html (뷰 템플릿 렌더링)
    }
}
```

<br>

> 동작 방식

1. `/welcome` 요청이 들어오면 welcome 메서드가 호출된다.
2. `Model`에 데이터를 담아 뷰 템플릿으로 전달한다.
3. `return "welcome"`은 `welcome.html` 파일을 찾아 HTML을 렌더링한다.
4. 클라이언트에게 최종 HTML 페이지가 응답 본문에 담겨 전송된다.

```html
HTTP/1.1 200 OK Content-Type: text/html

<!DOCTYPE html>
<html>
  <body>
    <h1>안녕하세요, 시은님!</h1>
  </body>
</html>
```

<br><br>

# 2. @RestController

> `@RestController`는 JSON, XML 등 데이터 자체를 반환할 때 사용되는 어노테이션이다.

`@Controller` + `@ResponseBody`의 기능을 합친 것으로, RESTful API를 구축할 때 사용된다.

<br>

> 특징

- 메서드의 반환값이 뷰 이름이 아닌 데이터 자체로 응답 본문에 들어간다.
- 주로 JSON 형식으로 데이터가 반환된다.

```java
@RestController
public class ApiController {

    @GetMapping("/api/welcome")
    public String welcome() {
        return "안녕하세요, 시은님!";
    }
}
```

<br>

> 동작 방식

1. `/api/welcome` 요청이 들어오면 `welcome` 메서드가 호출된다.
2. 메서드의 반환값(e.g., `"안녕하세요, 시은님!"`)이 직접 응답 본문으로 들어간다.
3. JSON 형식이나 문자열 그대로 클라이언트에 전달된다.

```
HTTP/1.1 200 OK
Content-Type: text/plain

안녕하세요, 시은님!
```

<br><br>

# 3. 차이점 정리

| 기능             | @Controller                     | @RestController             |
| ---------------- | ------------------------------- | --------------------------- |
| 반환 유형        | 뷰 템플릿 (JSP, HTML)           | JSON, XML, 문자열 등 데이터 |
| 사용 목적        | 웹 페이지 렌더링 (SSR)          | RESTful API 제공            |
| 데이터 반환 방식 | Model과 ViewResolver 사용       | @ResponseBody 자동 포함     |
| 사용 사례        | 전통적인 웹 애플리케이션 (HTML) | 클라이언트-서버 API 통신    |

- 웹 페이지를 직접 렌더링해야 한다면 -> `@Controller` 사용
- **데이터(API 응답)**를 반환해야 한다면 -> `@RestController` 사용

<br>
