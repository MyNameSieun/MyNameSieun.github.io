---
title: "[Spring] Model vs ModelAndView"
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

김영한 스프링 MVC 강의를 듣다가, 핸들러 어댑터가 `ModelAndView`를 반환하는 부분이 이해가 안갔다.

스프링의 Model과 ModelAndView 에 대해 자세히 알아보자.

---

<br>

# 1. 초기 Servlet 기반의 MVC의 문제점

> 스프링 MVC(Spring Model-View-Controller)에서는 컨트롤러가 클라이언트의 요청을 처리하고, 결과를 뷰(View)로 전달하는 역할을 한다.

초기 Servlet 기반의 MVC에서는 뷰 이름과 모델 데이터를 각각 따로 반환해야 했는데, 이는 다음과 같은 문제를 야기했다.

1. 서블릿(Servlet)에서는 HttpServletRequest와 HttpServletResponse를 통해 데이터를 전달하고, 뷰를 포워딩해야 했다.
2. 만약 JSP 뷰의 이름이 변경되면("`/exampleView.jsp`" → "`/newView.jsp`"), 모델 데이터를 설정하는 코드도 수정해야 할 수 있다.
3. 모델 데이터가 특정 뷰에 종속적이 되어, 만약 뷰를 교체하거나 변경할 때 코드의 많은 부분을 수정해야 하는 문제가 생긴다.

```java
RequestDispatcher dispatcher = request.getRequestDispatcher("/exampleView.jsp");  // 뷰 이름 설정
request.setAttribute("key", "value"); // 모델 데이터 설정
dispatcher.forward(request, response);
```

<br>

이처럼, 모델 데이터와 뷰의 전달이 분리되지 않아 유지보수에 어려움이 있었다.

<br><br>

# 2. ModelAndView의 등장

> 이를 해결하기 위해 `ModelAndView`가 도입되었다. (현재는 잘 사용 X)

- ModelAndView는 Spring MVC에서 **Model과 View를 함께 반환**할 수 있도록 만들어진 객체이다.
- ModelAndView는 주로 초기 스프링 MVC에서 사용되었고, 뷰(View) 이름과 모델(Model) 데이터를 한 번에 처리할 수 있는 장점이 있었다.

```java
@RequestMapping("/example")
public ModelAndView example() {
    ModelAndView mav = new ModelAndView("exampleView");  // 뷰 이름 설정
    mav.addObject("message", "안녕!");  // 모델에 데이터 추가

    return mav;
}
```

- `new ModelAndView("exampleView")`: exampleView라는 이름의 뷰를 렌더링
- message라는 이름으로 "안녕!" 데이터를 전달

<br>

> 최종 렌더링될 파일: `/WEB-INF/views/exampleView.jsp`

```html
<html>
  <body>
    <h1>${message}</h1>
  </body>
</html>
```

<br><br>

# 3. Model

> 현대 스프링에서는 ModelAndView 대신 `Model`과 `String`을 사용하여 뷰와 모델을 명확히 분리할 수 있다.

- Model은 데이터를 뷰(View)에 전달하기 위해 사용되는 컨테이너 역할을 한다.
- Model은 스프링이 자동으로 주입해주며, 뷰 이름은 단순히 **문자열(String)**로 반환한다.
- Model을 이용해 데이터를 추가하고, 뷰 이름은 문자열로 반환한다.
- 뷰 이름과 모델 데이터를 명확하게 분리할 수 있다는 점이 큰 장점이다.

```java
@GetMapping("/hello")
public String hello(Model model) {
    model.addAttribute("data", "안녕!"); // 모델에 데이터 추가
    return "helloView"; // 뷰 이름만 반환
}
```

데이터를 Model에, 뷰 이름은 String으로 따로 반환하여 역할을 명확히 분리하여, 유지보수성이 높아진다.

<br>

```
Model + String 방식에서도 Model에 데이터를 추가하고, 뷰 이름을 String으로 반환하는데,
이게 진짜 데이터와 뷰를 분리한 것인지 헷갈린다!
```

- `ModelAndView`는 하나의 객체에 모델과 뷰 정보를 동시에 담아 반환
- 반면, Model + String 방식에서는 Model 객체와 **뷰 이름(String)**을 따로 반환

```java
// ModelAndView 방식
@RequestMapping("/example")
public ModelAndView example() {
    ModelAndView mav = new ModelAndView("exampleView"); // 뷰 이름 설정
    mav.addObject("message", "안녕!"); // 모델 데이터 추가
    return mav; // 모델과 뷰를 하나의 객체에 담아 반환
}

// Model + String 방식
@GetMapping("/hello")
public String hello(Model model) { // 모델 데이터 추가
    model.addAttribute("data", "안녕!");
    return "helloView"; // 뷰 이름만 반환, 뷰 이름과 모델 데이터를 따로 처리
}
```

- ModelAndView는 뷰와 데이터를 한 객체에 담기 때문에, 뷰의 변경이 필요하면 모델 데이터도 같이 수정해야 할 가능성이 크다.
- 반면, Model + String 방식에서는 뷰 이름만 바꾸면 되고, 모델 데이터는 Model 객체에 담겨 있어서 별도의 수정이 필요 없다.

<br>

| 항목        | ModelAndView                   | Model + String                     |
| ----------- | ------------------------------ | ---------------------------------- |
| 데이터 전달 | `mav.addObject("key", value)`  | `model.addAttribute("key", value)` |
| 뷰 설정     | `new ModelAndView("viewName")` | `return "viewName";`               |
| 결합도      | 모델과 뷰가 한 객체에 묶임     | 모델과 뷰가 분리되어 전달          |
| 유연성      | 뷰 변경 시 코드 수정 필요      | 뷰 이름만 수정하면 됨              |

<br>
