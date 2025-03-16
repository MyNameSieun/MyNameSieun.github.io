---
title: "[Spring-MVC] 요청 매핑 (@Controller, @RestController, @RequestMapping)"
categories: [Spring]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

“요청이 왔을 때, 어떤 컨트롤러가 호출이 될건가?”

다양한 요청 매핑 방법에 대해 알아보자.

<br>

# 1. MappingController

## 1.1 @Controller

> `@Controller`는 주로 **뷰(View)를 반환**할 때 사용되는 어노테이션이다.

JSP, Thymeleaf, Freemarker 등의 뷰 템플릿을 이용해 HTML을 렌더링할 때 사용된다.

<br>

> 특징

- 메서드에서 return "뷰 이름"을 반환하면 해당 이름의 HTML 파일을 렌더링한다.
- Model 객체를 이용해 데이터를 뷰에 전달할 수 있다.
- 주로 서버 사이드 렌더링(SSR) 방식에 사용된다.

```java
@Controller
public class MappingController {

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

<br>

## 1.2 @RestController

> `@RestController`는 JSON, XML 등 데이터 자체를 반환할 때 사용되는 어노테이션이다.

`@Controller` + `@ResponseBody`의 기능을 합친 것으로, RESTful API를 구축할 때 사용된다.

<br>

> 특징

- 메서드의 반환값이 뷰 이름이 아닌 데이터 자체로 응답 본문에 들어간다.
- 주로 JSON 형식으로 데이터가 반환된다.

```java
@Slf4j
@RestController
public class MappingController  {

    @GetMapping("/api/welcome")
    public String welcome() {
        log.info("welcome");
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

<br>

## 1.3 @Controller vs @RestController

- `@Controller` 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 렌더링 된다.
- 하지만, `@RestController` 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다. (String이 바로 반환)

<br>

> 차이점 정리

- 웹 페이지를 직접 렌더링해야 한다면 -> `@Controller` 사용
- **데이터(API 응답)** 를 반환해야 한다면 -> `@RestController` 사용

| 기능             | @Controller                     | @RestController             |
| ---------------- | ------------------------------- | --------------------------- |
| 반환 유형        | 뷰 템플릿 (JSP, HTML)           | JSON, XML, 문자열 등 데이터 |
| 사용 목적        | 웹 페이지 렌더링 (SSR)          | RESTful API 제공            |
| 데이터 반환 방식 | Model과 ViewResolver 사용       | @ResponseBody 자동 포함     |
| 사용 사례        | 전통적인 웹 애플리케이션 (HTML) | 클라이언트-서버 API 통신    |

<br><br>

# 2. HTTP 메서드 매핑

## 2.1 @RequestMapping

> `@RequestMapping` 에 method 속성으로 HTTP 메서드를 지정하지 않으면 HTTP 메서드와 무관하게 호출된다.

만약 여기에 POST 요청을 하면 스프링 MVC는 HTTP 405 상태코드(Method Not Allowed)를 반환한다.

```java
@RequestMapping(value = "/new-form", method = RequestMethod.GET)
public String newForm() {
    return "new-form";
}
```

- `@RequestMapping`을 사용하여 URL 경로와 HTTP 메서드를 함께 정의
- `method = RequestMethod.GET`과 같이 명시적으로 GET 메서드를 지정해야 함
- URL 경로와 메서드 유형을 모두 작성해야 하므로 코드가 길어질 수 있음

<br>

## 2.2 HTTP 메시지 매핑 축약

```java
/**
* 편리한 축약 애노테이션
* @GetMapping
* @PostMapping
* @PutMapping
* @DeleteMapping
* @PatchMapping
*/
@GetMapping("/new-form")
public String newForm() {
    return "new-form";
}
```

- `@GetMapping`은 GET 요청을 위한 전용 어노테이션으로, 가독성이 높아짐
- HTTP 메서드(GET)를 명시할 필요 없이 경로만 작성하면 됨
- 코드가 간결해지고 실수를 줄일 수 있음

<br>

> 메서드 레벨과의 조합으로 URL 경로 중복 최소화

클래스 레벨에 `@RequestMapping`를 두면 메서드 레벨과 조합이 되서 중복을 해결할 수 있다.

<br>

[ 사용 전 ]

```java
@RestController
public class UserController {

    @GetMapping("/user/list")
    public String getUserList() {
        return "User List";
    }

    @PostMapping("/user/create")
    public String createUser() {
        return "Create User";
    }
}
```

- 각 메서드마다 `/user` 경로를 반복해서 작성해야 함
- URL 경로가 길어질수록 유지보수가 어려움

<br>

[ 사용 후 ]

```java
@RestController
@RequestMapping("/user")
public class UserController {

    @GetMapping("/list")
    public String getUserList() {
        return "User List";
    }

    @PostMapping("/create")
    public String createUser() {
        return "Create User";
    }
}
```

- 클래스 레벨에서 공통 경로인 `/user`를 설정
- 메서드 레벨에서는 공통 경로를 제외한 엔드포인트만 정의
- URL 경로 중복을 줄이고, 유지보수성 향상

<br><br>

# 3. PathVariable 사용

## 3.1 PathVariable(경로 변수) 사용

```java
    /**
     * PathVariable 사용
     * 변수명이 같으면 생략 가능
     * @PathVariable("userId") String userId -> @PathVariable String userId
     */
    @GetMapping("/mapping/{userId}")
    public String mappingPath(@PathVariable("userId") String data) {
        log.info("mappingPath userId={}", data);
        return "ok";
    }
```

실행: [http://localhost:8080/mapping/userA](http://localhost:8080/mapping/userA)

<br>

> 설명

- 최근 HTTP API는 `/mapping/userA`과 같이 리소스 경로에 식별자를 넣는 스타일을 선호한다.
- `@RequestMapping` 은 URL 경로를 템플릿화 할 수 있는데, `@PathVariable` 을 사용하면 매칭 되는 부분을 편리하게 조회할 수 있다.
  - 예제에서 사용한 `@GetMapping("/mapping/{userId}")`는 `@RequestMapping`의 축약형
- 📌 `@PathVariable` 의 이름과 파라미터 이름이 같으면 생략할 수 있다.

<br>

## 3.2 PathVariable 사용 - 다중

```java
    /**
     * PathVariable 사용 다중
     */
    @GetMapping("/mapping/users/{userId}/orders/{orderId}")
    public String mappingPath(@PathVariable String userId, @PathVariable Long orderId) {
        log.info("mappingPath userId={}, orderId={}", userId, orderId);
        return "ok";
    }
```

실행: [http://localhost:8080/mapping/users/userA/orders/100](http://localhost:8080/mapping/users/userA/orders/100)

<br>

## 3.3 조건 매핑

### 3.3.1 특정 파라미터 조건 매핑

> 특정 파라미터가 있거나 없는 조건을 추가할 수 있다. 잘 사용하지는 않는다.

```java
@Slf4j
@RestController
public class MappingController {
		 /**
		 * 기본 요청
		 * 둘다 허용 /hello-basic, /hello-basic/
		 * HTTP 메서드 모두 허용 GET, HEAD, POST, PUT, PATCH, DELETE
		 */
    @RequestMapping("/hello-basic")
    public String helloBasic() {
        log.info("helloBasic");
        return "ok";
    }
}
```

파라미터에 `mode=debug` 가 있어야 실행이 된다.

실행: [http://localhost:8080/mapping-param?mode=debug](http://localhost:8080/mapping-param?mode=debug)

<br>

### 3.3.2 특정 헤더 조건 매핑

> 파라미터 매핑과 비슷하지만, HTTP 헤더를 사용한다.

```java
/**
 * 특정 헤더로 추가 매핑
 * headers="mode",
 * headers="!mode"
 * headers="mode=debug"
 * headers="mode!=debug" (! = )
 */
@GetMapping(value = "/mapping-header", headers = "mode=debug")
public String mappingHeader() {
 log.info("mappingHeader");
 return "ok";
}
```

Postman으로 테스트 해야 한다.

![](/assets/images/2025/2025-03-10-19-35-08.png)

<br>

### 3.3.3 미디어 타입 조건 매핑

> HTTP 요청 Content-Type, consume

Content-Type에 따라 다르게 처리하고 싶다면, headers가 아닌 consume 를 사용해야 한다.

```java
    /**
     * Content-Type 헤더 기반 추가 매핑 Media Type
     * consumes="application/json"
     * consumes="!application/json"
     * consumes="application/*"
     * consumes="*\/*"
     * MediaType.APPLICATION_JSON_VALUE
     */
    @PostMapping(value = "/mapping-consume", consumes = "application/json")
    public String mappingConsumes() {
        log.info("mappingConsumes");
        return "ok";
    }
```

Postman으로 테스트 해야 한다.

HTTP 요청의 Content-Type 헤더를 기반으로 미디어 타입으로 매핑한다.<br>
만약 맞지 않으면 HTTP 415 상태코드(Unsupported Media Type)을 반환한다.

<br>

> HTTP 요청 Accept, produce

```java
    /**
     * Accept 헤더 기반 Media Type
     * produces = "text/html"
     * produces = "!text/html"
     * produces = "text/*"
     * produces = "*\/*"
     */
    @PostMapping(value = "/mapping-produce", produces = "text/html")
    public String mappingProduces() {
        log.info("mappingProduces");
        return "ok";
    }
```

HTTP 요청의 Accept 헤더를 기반으로 미디어 타입으로 매핑한다.<br>
만약 맞지 않으면 HTTP 406 상태코드(Not Acceptable)을 반환한다.

<br>

```java
produces = "text/plain"
produces = {"text/plain", "application/*"}
produces = MediaType.TEXT_PLAIN_VALUE // 왠만하면 상수로 쓰는 것이 좋다!
produces = "text/plain;charset=UTF-8"
```

<br><br>

# 4. 요청 매핑 API 예시

회원 관리를 HTTP API로 만든다 생각하고 매핑을 어떻게 하는지 알아보자.

(실제 데이터가 넘어가는 부분은 생략하고 URL 매핑만)

<br>

## 4.1 회원 관리 API

- 회원 목록 조회: GET `/users`
- 회원 등록: POST `/users`
- 회원 조회: GET `/users/{userId}`
- 회원 수정: PATCH `/users/{userId}`
- 회원 삭제: DELETE `/users/{userId}`

<br>

## 4.2 MappingClassController

```java
package hello.springmvc.basic.requestmapping;

@RestController
@RequestMapping("/users")
public class MappingClassController {
    /**
     * GET /users
     */
    @GetMapping
    public String users() {
        return "get users";
    }

    /**
     * POST /users
     */
    @PostMapping
    public String addUser() {
        return "post user";
    }

    /**
     * GET /users/{userId}
     */
    @GetMapping("/{userId}")
    public String findUser(@PathVariable String userId) {
        return "get userId=" + userId;
    }

    /**
     * PATCH /users/{userId}
     */
    @PatchMapping("/{userId}")
    public String updateUser(@PathVariable String userId) {
        return "update userId=" + userId;
    }

    /**
     * DELETE /users/{userId}
     */
    @DeleteMapping("/{userId}")
    public String deleteUser(@PathVariable String userId) {
        return "delete userId=" + userId;
    }
}
```

Postman으로 테스트

- `@RequestMapping("/mapping/users")`
  - 클래스 레벨에 매핑 정보를 두면 메서드 레벨에서 해당 정보를 조합해서 사용

<br>
