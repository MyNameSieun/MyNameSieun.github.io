---
title: "[Spring-MVC] Spring MVC 웹페이지 만들기"
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

[[김영한 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술]](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1)을 듣고 정리한 글입니다.
{: .notice}

---

<br>

# 1. 프로젝트 생성

## 1.1 스프링 프로젝트 생성

> [스프링 부트 스타터 사이트](https://start.spring.io)로 이동해서 스프링 프로젝트 생성

- **프로젝트 선택**
  - Project: Gradle Project
  - Language: Java
  - Spring Boot: 3.4.x
- **Project Metadata**
  - Group: hello
  - Artifact: item-service
  - Name: item-service
  - Package name: hello.itemservice (특수기호 주의!)
    Packaging: **Jar**
  - Java: 17
- **Dependencies**
  - Spring Web, Thymeleaf, Lombok

![](/assets/images/2025/2025-03-11-09-08-23.png)

<br>

> 그 후 `build.gradle` 열어주기

`build.gradle`은 Gradle 빌드 도구에서 사용하는 설정 파일로, 주로 프로젝트의 의존성 관리와 빌드 설정을 정의하는 데 사용된다.

![](/assets/images/2025/2025-03-11-09-11-54.png)

<br>

> Settings > Annotation Processors에서 저 체크표시를 켜줘야 롬복이 적용된다.

![](/assets/images/2025/2025-03-11-09-12-44.png)

<br>

> Settings > Gradle 에서 위 두 개 IntelliJ IDEA로 설정하면 조금 더 빨리 실행할 수 있다.

(그런데 스프링 부트 3.2 파라미터 이름 인식 문제로 오류가 발생하니 그냥 Gradle로 설정하자(Gradle는 defult 값이다.))

![](/assets/images/2025/2025-03-11-09-14-07.png)

<br>

## 1.2 Welcome 페이지 추가

> 스프링부트는 `src/resource/static/index.html` 에 있는 페이지를 첫 화면으로 렌더링한다.

`/resources/static/index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li>
        상품 관리
        <ul>
          <li><a href="/basic/items">상품 관리 - 기본</a></li>
        </ul>
      </li>
    </ul>
  </body>
</html>
```

- 동작 확인
  - 기본 메인 클래스 실행( `SpringmvcApplication.main()` )
  - [http://localhost:8080](http://localhost:8080/) 호출해서 Welcome 페이지가 나오면 성공

<br><br>

# 2. 요구사항 분석

> 상품을 관리할 수 있는 서비스를 만들어보자.

- **상품 도메인 모델**

  - 상품 ID
  - 상품명
  - 가격
  - 수량

- **상품 관리 기능**

  - 상품 목록
  - 상품 상세
  - 상품 등록
  - 상품 수

- **서비스 화면**<br><br>
  ![](/assets/images/2025/2025-03-11-09-21-04.png)

- **서비스 제공 흐름**<br><br>
  ![](/assets/images/2025/2025-03-11-09-22-16.png)

<br><br>

# 3. 상품 서비스 HTML

다음 파일들을 경로에 넣고 잘 동작하는지 확인해보자.

## 3.1 부트스트랩

- HTML을 편리하게 개발하기 위해 부트스트랩 사용했다.
- 부트스트랩 공식 사이트: [https://getbootstrap.com](https://getbootstrap.com/)
- 부트스트랩을 다운로드 받고 압축을 풀자.
  - 이동: [https://getbootstrap.com/docs/5.0/getting-started/download/](https://getbootstrap.com/docs/5.0/getting-started/download/)
  - Compiled CSS and JS 항목을 다운로드하자.
  - 압축을 출고 `bootstrap.min.css` 를 복사해서 다음 폴더에 추가하자
  - `resources/static/css/bootstrap.min.css`

<br>

## 3.2 HTML 파일

- HTML 파일들을 `/resources/static` 에 넣어두었기 때문에 스프링 부트가 정적 리소스를 제공한다.
- 화면 UI를 미리 확인할 수 있도록 하기 위해 정적 HTML 파일을 먼저 만드는 것이다.

  - 상품 목록 HTML `resources/static/html/items.html`
  - 상품 상세 HTML `resources/static/html/item.html`
  - 상품 등록 폼 HTML `resources/static/html/addForm.html`
  - 상품 수정 폼 HTML `resources/static/html/editForm.html`

<br>

그런데 정적 리소스여서 **해당 파일을 탐색기를 통해 직접 열어**도 동작하는 것을 확인할 수 있다.

이렇게 정적 리소스가 공개되는 `/resources/static` 폴더에 HTML을 넣어두면, 실제 서비스에서도 공개된다.

서비스를 운영한다면 지금처럼 공개할 필요없는 HTML을 두는 것은 주의하자.

<br>

💡 추후 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하여 템플릿 엔진을 적용하여 동적으로 변환할 것이다.

<br>

> 상품 목록 HTML `resources/static/html/items.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link href="../css/bootstrap.min.css" rel="stylesheet" />
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
            <tr>
              <td><a href="item.html">1</a></td>
              <td><a href="item.html">테스트 상품1</a></td>
              <td>10000</td>
              <td>10</td>
            </tr>
            <tr>
              <td><a href="item.html">2</a></td>
              <td><a href="item.html">테스트 상품2</a></td>
              <td>20000</td>
              <td>20</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

<br>

> 상품 상세 HTML `resources/static/html/item.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link href="../css/bootstrap.min.css" rel="stylesheet" />
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 상세</h2>
      </div>
      <div>
        <label for="itemId">상품 ID</label>
        <input
          type="text"
          id="itemId"
          name="itemId"
          class="form-control"
          value="1"
          readonly
        />
      </div>
      <div>
        <label for="itemName">상품명</label>
        <input
          type="text"
          id="itemName"
          name="itemName"
          class="form-control"
          value="상품A"
          readonly
        />
      </div>
      <div>
        <label for="price">가격</label>
        <input
          type="text"
          id="price"
          name="price"
          class="form-control"
          value="10000"
          readonly
        />
      </div>
      <div>
        <label for="quantity">수량</label>
        <input
          type="text"
          id="quantity"
          name="quantity"
          class="form-control"
          value="10"
          readonly
        />
      </div>
      <hr class="my-4" />
      <div class="row">
        <div class="col">
          <button
            class="w-100 btn btn-primary btn-lg"
            onclick="location.href='editForm.html'"
            type="button"
          >
            상품 수정
          </button>
        </div>
        <div class="col">
          <button
            class="w-100 btn btn-secondary btn-lg"
            onclick="location.href='items.html'"
            type="button"
          >
            목록으로
          </button>
        </div>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

<br>

> 상품 등록 폼 HTML `resources/static/html/addForm.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link href="../css/bootstrap.min.css" rel="stylesheet" />
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 등록 폼</h2>
      </div>
      <h4 class="mb-3">상품 입력</h4>
      <form action="item.html" method="post">
        <div>
          <label for="itemName">상품명</label>
          <input
            type="text"
            id="itemName"
            name="itemName"
            class="formcontrol"
            placeholder="이름을 입력하세요"
          />
        </div>
        <div>
          <label for="price">가격</label>
          <input
            type="text"
            id="price"
            name="price"
            class="form-control"
            placeholder="가격을 입력하세요"
          />
        </div>
        <div>
          <label for="quantity">수량</label>
          <input
            type="text"
            id="quantity"
            name="quantity"
            class="formcontrol"
            placeholder="수량을 입력하세요"
          />
        </div>
        <hr class="my-4" />
        <div class="row">
          <div class="col">
            <button class="w-100 btn btn-primary btn-lg" type="submit">
              상품 등 록
            </button>
          </div>
          <div class="col">
            <button
              class="w-100 btn btn-secondary btn-lg"
              onclick="location.href='items.html'"
              type="button"
            >
              취소
            </button>
          </div>
        </div>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

<br>

> 상품 수정 폼 HTML `resources/static/html/editForm.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link href="../css/bootstrap.min.css" rel="stylesheet" />
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 수정 폼</h2>
      </div>
      <form action="item.html" method="post">
        <div>
          <label for="id">상품 ID</label>
          <input
            type="text"
            id="id"
            name="id"
            class="form-control"
            value="1"
            readonly
          />
        </div>
        <div>
          <label for="itemName">상품명</label>
          <input
            type="text"
            id="itemName"
            name="itemName"
            class="formcontrol"
            value="상품A"
          />
        </div>
        <div>
          <label for="price">가격</label>
          <input
            type="text"
            id="price"
            name="price"
            class="form-control"
            value="10000"
          />
        </div>
        <div>
          <label for="quantity">수량</label>
          <input
            type="text"
            id="quantity"
            name="quantity"
            class="formcontrol"
            value="10"
          />
        </div>
        <hr class="my-4" />
        <div class="row">
          <div class="col">
            <button class="w-100 btn btn-primary btn-lg" type="submit">
              저장
            </button>
          </div>
          <div class="col">
            <button
              class="w-100 btn btn-secondary btn-lg"
              onclick="location.href='item.html'"
              type="button"
            >
              취소
            </button>
          </div>
        </div>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

<br><br>

# 4. 상품 도메인 개발

## 4.1 Item - 상품 객체

> `hello.itemservice.domain.item` 에 `Item` 클래스 생성

```java
package hello.item_service.domain.item;

import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class Item {
    private Long id;
    private String itemName;
    private Integer price;
    private Integer quantity;

    public Item(){}

    // 생성자 단축키 (Alt + Insert)
    public Item(String itemName, Integer price, Integer quantity) {
        this.itemName = itemName;
        this.price = price;
        this.quantity = quantity;
    }
}
```

- `Integer` 를 사용하는 이유는 수량이 null일 수도 있기 때문, `Int` 를 사용하면 0이라도 들어가야함
- 핵심 도메인에서는 `@Data` 보다 `@Getter` , `@Setter` 로 명확히 하는 것이 좋다.

<br>

## 4.2 ItemRepository - 상품 저장소

> `hello.itemservice.domain.item` 에 `ItemRepository` 클래스 생성

`@Repository`어노테이션을 추가하면 Spring이 ItemRepository를 Bean으로 등록한다.

```java
package hello.itemservice.domain.item;

@Repository
public class ItemRepository {

    private static final Map<Long, Item> store = new HashMap<>(); // static 사용
    private static long sequence = 0L; // static 사용

    public Item save(Item item){
        item.setId(++sequence);
        store.put(item.getId(), item);
        return item;
    }

    public Item findById(Long id){
        return store.get(id);
    }

    public List<Item> findAll(){
        return new ArrayList<>(store.values());
    }

    public void update(Long itemId, Item updateParam) {
        Item findItem = findById(itemId);
        findItem.setItemName(updateParam.getItemName());
        findItem.setPrice(updateParam.getPrice());
        findItem.setQuantity(updateParam.getQuantity());
    }

    public void clearStore(){
        store.clear();
    }
}
```

<br>

## 4.3 ItemRepositoryTest - 상품 저장소 테스트

> test 폴더 아래에 `hello.itemservice.domain.item` 에 `ItemRepositoryTest` 를 생성하여 테스트를 진행하자.

테스트 단축키 : Ctrl + Shift + T

```java
package hello.itemservice.domain.item;

class ItemRepositoryTest {
    ItemRepository itemRepository = new ItemRepository();

    @AfterEach
    void afterEach() {
        itemRepository.clearStore();
    }


    @Test
    void save() {
        //given
        Item item = new Item("itemA", 10000, 10);

        //when
        Item savedItem = itemRepository.save(item);

        //then
        Item findItem = itemRepository.findById(item.getId());
        Assertions.assertThat(findItem).isEqualTo(savedItem);
    }

    @Test
    void findAll() {
        //given
        Item item1 = new Item("item1", 10000, 10);
        Item item2 = new Item("item2", 20000, 20);

        itemRepository.save(item1);
        itemRepository.save(item2);

        //when
        List<Item> result = itemRepository.findAll();

        //then
        Assertions.assertThat(result.size()).isEqualTo(2);
        Assertions.assertThat(result).contains(item1, item2);
    }

    @Test
    void updateItem() {
        //given
        Item item = new Item("item1", 10000, 10);
        Item savedItem = itemRepository.save(item);
        Long itemId = savedItem.getId();

        //when
        Item updateParam = new Item("item2", 20000, 30);
        itemRepository.update(itemId, updateParam);
        Item findItem = itemRepository.findById(itemId);

        //then
        Assertions.assertThat(findItem.getItemName()).isEqualTo(updateParam.getItemName());
        Assertions.assertThat(findItem.getPrice()).isEqualTo(updateParam.getPrice());
        Assertions.assertThat(findItem.getQuantity()).isEqualTo(updateParam.getQuantity());
    }
}
```

> @AfterEach:

- 한번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있다.
- 이렇게 되면 다음 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있다. `@AfterEach`를 사용하면 각 테스트가 종료될 때 마다 이 기능을 실행한다.
- 여기서는 메모리 DB에 저장된 데이터를 삭제한다.

테스트는 각각 독립적으로 실행되어야 한다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아니다.

<br><br>

# 5. 상품 목록

## 5.1 컨트롤러

> `hello.itemservice.web.item.basic` 에 `BasicItemController` 생성

`itemRepository` 에서 모든 상품을 조회한 다음에 모델에 담는다. 그리고 뷰 템플릿을 호출한다.

```java
package hello.itemservice.web.item.basic;

@Controller
@RequestMapping("/basic/items")
@RequiredArgsConstructor
public class BasicItemController {
		// Spring이 ItemRepository를 자동으로 빈(bean)으로 등록하고 주입
    private final ItemRepository itemRepository;

    @GetMapping
    public String items(Model model) {
        List<Item> items = itemRepository.findAll();
        model.addAttribute("items", items);
        return "basic/items";
    }

    /**
     * 테스트용 데이터 추가
     */
    @PostConstruct
    public void init() {
        itemRepository.save(new Item("감자", 500, 10));
        itemRepository.save(new Item("고구마", 700, 20));
    }
}
```

- 테스트용 데이터 추가
  - 테스트용 데이터가 없으면 회원 목록 기능이 정상 동작하는지 확인하기 어렵다.
  - `@PostConstruct` : 해당 빈의 의존관계가 모두 주입되고 나면 초기화 용도로 호출된다.
  - 여기서는 간단히 테스트용 테이터를 넣기 위해서 사용했다.

<br>

### 5.1.1 의존관계 주입 리마인드

> 의존성 (Dependency)

- 위 코드에서 BasicItemController는 ItemRepository가 있어야만 동작할 수 있다.
- 이때 BasicItemController는 ItemRepository에 의존하고 있다고 말한다.

<br>

> 기존의 의존성 생성 방식 (안 좋은 예)

- ItemRepository를 직접 생성하고 있다.
- 만약 ItemRepository의 구현체가 바뀌거나 테스트 시 Mock 객체를 사용해야 한다면, 코드를 변경해야 한다.
- 결합도가 높아지고 코드 변경 시 유연하지 않다.<br><br>

```java
public class BasicItemController {
private ItemRepository itemRepository = new ItemRepository(); // 직접 생성

    public void getItems() {
        List<Item> items = itemRepository.findAll();
    }
}
```

<br>

> 의존관계 주입의 장점: 결합도 감소

- 이제 BasicItemController는 ItemRepository의 구체적인 구현체를 몰라도 된다.
- 외부에서 어떤 구현체(MemoryItemRepository, DatabaseItemRepository)를 주입해도 동작한다.

```java
public class BasicItemController {
    private final ItemRepository itemRepository;

    public BasicItemController(ItemRepository itemRepository) {
        this.itemRepository = itemRepository;
    }
}
```

<br>

> Spring에서의 DI 방법

다양한 방법이 있지만, 생성자 주입이 가장 권장된다.

```java
public BasicItemController(ItemRepository itemRepository) {
  this.itemRepository = itemRepository;
}
```

- 위 코드처럼 생성자가 딱 1개만 있으면 스프링이 해당 생성자에 `@Autowired` 로 의존관계를 주입해준다.
- `@RequiredArgsConstructor`를 사용하면 Lombok이 자동으로 위 생성자를 만들어준다.

<br>

### 5.1.2 @RequiredArgsConstructor

> `final` 이 붙은 멤버변수만 사용해서 생성자를 자동으로 만들어준다.

```java
@RequiredArgsConstructor
public class BasicItemController {
    private final ItemRepository itemRepository;
}
```

- 따라서 final 키워드를 빼면 안된다! 그러면 `ItemRepository` 의존관계 주입이 안된다.
- 스프링 핵심원리 - 기본편 강의 참고

<br>

> 만약 `final`이 없는 필드가 있다면?

```java
@RequiredArgsConstructor
public class BasicItemController {
    private final ItemRepository itemRepository;
    private String name; // final이 없음
}
```

이 경우 Lombok은 `name` 필드는 무시하고 `final`이 있는 필드만 포함한 생성자를 자동 생성

<br>

> 아래와 같이 생성자 위에 `@Autowired` 를 사용하여 의존성 주입을 해도 된다.

```java
    @Autowired
    public BasicItemController(ItemRepository itemRepository) {
        this.itemRepository = itemRepository;
    }
```

<br>

### 5.1.3 @Autowired vs @RequiredArgsConstructor 차이점

| 어노테이션               | 역할                                                     | 사용 방식                                              | 특징                                |
| ------------------------ | -------------------------------------------------------- | ------------------------------------------------------ | ----------------------------------- |
| @Autowired               | 생성자 기반 의존성 주입                                  | 생성자에 직접 붙이거나 생략 가능 (Spring 4.3 이상)     | 생성자가 하나만 있을 경우 생략 가능 |
| @RequiredArgsConstructor | `final`이 붙은 필드들을 매개변수로 받는 생성자 자동 생성 | 클래스 위에 붙이면 롬복(Lombok)이 자동으로 생성자 생성 | 불변 객체 보장, 코드 간결           |

<br>

```
@Autowired를 사용하면 생성자가 하나만 있을 경우 생략이 가능하다.
생성자가 하나만 있으면 스프링이 자동으로 @Autowired를 적용해 주기 때문이다.

그렇다면 다시 처음으로 돌아와서, 상품 목록 컨트롤러에
@Autowired @RequiredArgsConstructor 둘 다 필요 없는 게 아닌가?
```

- 필요 없다! 상품 목록 컨트롤러에는 생성자가 하나이므로 스프링이 자동으로 `@Autowired`를 적용해준다.
- 따라서 `@Autowired` `@RequiredArgsConstructor`가 없어도 동작한다.

<br>

```
그러면 위 코드에서 @RequiredArgsConstructor를 사용한 이유가 무엇일까?
```

- 코드를 간결하게 만들기 위해서다.
- 생성자가 하나만 있을 경우 `@Autowired`와 `@RequiredArgsConstructor`를 생략할 수 있지만, 생성자 자체는 반드시 있어야 한다. <- (이 부분이 헷갈렸다.)
- Lombok을 쓰면 `@RequiredArgsConstructor`로 생성자를 자동 생성이 가능하다.

<br><br>

## 5.2 뷰 템플릿

> items.html 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하고 다음과 같이 수정하자 (동적으로 만들기 위함)

- `/resources/static/html/items.html` → 복사 → `/resources/templates/basic/items.html`
- 아래는 타임리프를 사용하였다. 타임리프에 대한 자세한 내용은 [[타임리프(Thymeleaf)]](https://mynamesieun.github.io/spring/%ED%83%80%EC%9E%84%EB%A6%AC%ED%94%84(Thymeleaf)/) 포스팅을 참고하자.

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

![](/assets/images/2025/2025-03-11-13-58-12.png)

<br><br>

# 6. 상품 상세

## 6.1 컨트롤러

> BasicItemController에 추가

```java
@GetMapping("/{itemId}")
public String item(@PathVariable Long itemId, Model model){
    Item item = itemRepository.findById(itemId);
    model.addAttribute("item", item);
    return "basic/item";
}
```

`PathVariable` 로 넘어온 상품ID로 상품을 조회하고, 모델에 담아둔다. 그리고 뷰 템플릿을 호출한다.

<br>

## 6.2 뷰 템플릿

> 상품 상세 뷰 (`/resources/templates/basic/item.html`)

- 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하고 다음과 같이 수정하자.
  - `/resources/static/html/item.html` → 복사 → `/resources/templates/basic/item.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="utf-8" />
    <link th:href="@{/css/bootstrap.min.css}" rel="stylesheet" />
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 상세</h2>
      </div>

      <div>
        <label for="itemId">상품 ID</label>
        <input
          type="text"
          id="itemId"
          name="itemId"
          class="form-control"
          th:value="${item.id}"
          readonly
        />
      </div>

      <div>
        <label for="itemName">상품명</label>
        <input
          type="text"
          id="itemName"
          name="itemName"
          class="form-control"
          th:value="${item.itemName}"
          readonly
        />
      </div>

      <div>
        <label for="price">가격</label>
        <input
          type="text"
          id="price"
          name="price"
          class="form-control"
          th:value="${item.price}"
          readonly
        />
      </div>

      <div>
        <label for="quantity">수량</label>
        <input
          type="text"
          id="quantity"
          name="quantity"
          class="form-control"
          th:value="${item.quantity}"
          readonly
        />
      </div>

      <hr class="my-4" />

      <div class="row">
        <div class="col">
          <button
            class="w-100 btn btn-primary btn-lg"
            th:onclick="|location.href='@{/basic/items/{itemId}/edit(itemId=${item.id})}'|"
            type="button"
          >
            상품 수정
          </button>
        </div>

        <div class="col">
          <button
            class="w-100 btn btn-secondary btn-lg"
            th:onclick="|location.href='@{/basic/items}'|"
            type="button"
          >
            목록으로
          </button>
        </div>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

![](/assets/images/2025/2025-03-11-15-57-42.png)

<br>

> 예제 데이터 (item.id가 2인 경우)

```html
<th:onclick
  ="|location.href='@{/basic/items/{itemId}/edit(itemId=${item.id})}'|"
>
</th:onclick>
```

[http://localhost:8080/basic/items/2/edit](http://localhost:8080/basic/items/2/edit) 같이 접근 가능

<br>

## 6.3 스프링 부트 3.2 파라미터 이름 인식 문제

> 스프링 부트 3.2부터 자바 컴파일러에 `-parameters` 옵션을 넣어주어야 애노테이션의 이름을 생략할 수 있다.

발생하는 예외

```
java.lang.IllegalArgumentException: Name for argument of type [java.lang.String]
not specified, and parameter name information not found in class file either.
```

주로 다음 두 애노테이션에서 문제가 발생한다. `@RequestParam `, `@PathVariable`

<br>

> 나는 아래와 같이 해결했다.

- `Gradle`을 사용해서 빌드하고 실행한다.
- 참고로 이 문제는 Build, Execution, Deployment -> Build Tools -> Gradle에서 Build and run using를 IntelliJ IDEA로 선택한 경우에만 발생한다.
- Gradle로 선택한 경우에는 Gradle이 컴파일 시점에 해당 옵션을 자동으로 적용해준다.

<br><br>

# 7. 상품 등록 폼

## 7.1 컨트롤러

BasicItemController에 추가

```java
    @GetMapping("/add")
    public String addForm() {
        return "basic/addForm";
    }
```

상품 등록 폼은 단순히 뷰 템플릿만 호출한다.

<br>

## 7.2 뷰 템플릿

- 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하고 다음과 같이 수정하자.
- `/resources/static/html/addForm.html` → 복사 → `/resources/templates/basic/addForm.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="utf-8" />
    <link th:href="@{/css/bootstrap.min.css}" rel="stylesheet" />
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 등록 폼</h2>
      </div>

      <h4 class="mb-3">상품 입력</h4>

      <form action="item.html" th:action method="post">
        <div>
          <label for="itemName">상품명</label>
          <input
            type="text"
            id="itemName"
            name="itemName"
            class="form-control"
            placeholder="이름을 입력하세요"
          />
        </div>

        <div>
          <label for="price">가격</label>
          <input
            type="text"
            id="price"
            name="price"
            class="form-control"
            placeholder="가격을 입력하세요"
          />
        </div>

        <div>
          <label for="quantity">수량</label>
          <input
            type="text"
            id="quantity"
            name="quantity"
            class="form-control"
            placeholder="수량을 입력하세요"
          />
        </div>

        <hr class="my-4" />

        <div class="row">
          <div class="col">
            <button class="w-100 btn btn-primary btn-lg" type="submit">
              상품 등록
            </button>
          </div>

          <div class="col">
            <button
              class="w-100 btn btn-secondary btn-lg"
              th:onclick="|location.href='@{/basic/items}'|"
              type="button"
            >
              취소
            </button>
          </div>
        </div>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

![](/assets/images/2025/2025-03-11-15-56-47.png)

<br>

> **속성 변경 - th:action**

- `th:action`
- HTML form에서 `action` 에 값이 없으면 현재 URL에 데이터를 전송한다.
- 상품 등록 폼의 URL과 실제 상품 등록을 처리하는 URL을 똑같이 맞추고 HTTP 메서드로 두 기능을 구분한다.
  - 상품 등록 폼: GET `/basic/items/add`
  - 상품 등록 처리: POST `/basic/items/add`
- 이렇게 하면 하나의 URL로 등록 폼과, 등록 처리를 깔끔하게 처리할 수 있다.

> **취소**

취소시 상품 목록으로 이동한다.

```html
th:onclick="|location.href='@{/basic/items}'|"
```

<br><br>

# 8. 상품 등록 처리

> 이제 상품 등록 폼에서 전달된 데이터로 실제 상품을 등록 처리해보자.

상품 등록 폼은 다음 방식으로 서버에 데이터를 전달한다.

- POST - HTML Form
  - `content-type: application/x-www-form-urlencoded`
  - 메시지 바디에 쿼리 파리미터 형식으로 전달 `itemName=itemA&price=10000&quantity=10`
  - e.g., 회원 가입, 상품 주문, HTML Form 사용

요청 파라미터 형식을 처리해야 하므로 먼저 `@RequestParam` 을 사용하자

<br>

```
아래 코드들은 상품 등록 완료 후 새로고침 시 상품이 계속해서 중복 등록되는 문제가 있다.
따라서 목차 (10 - 12)을 참고하도록 하자.
```

<br>

## 8.1 addItemV1 - @RequestParam

addItemV1 - BasicItemController에 추가

```java
    @PostMapping("/add")
    public String save(@RequestParam String itemName,
                       @RequestParam int price,
                       @RequestParam Integer quantity,
                       Model model){

        Item item = new Item();
        item.setItemName(itemName);
        item.setPrice(price);
        item.setQuantity(quantity);

        itemRepository.save(item);

        model.addAttribute("item", item);

        return "basic/item";
    }
```

- 먼저 `@RequestParam String itemName` : itemName 요청 파라미터 데이터를 해당 변수에 받는다.
  - `view`의 `input` 태그의 `name` 속성과 `@RequestParam` 변수명이 일치해야 한다.
- `Item` 객체를 생성하고 `itemRepository` 를 통해서 저장한다.
- 저장된 `item` 을 모델에 담아서 뷰에 전달한다.

⭐ 상품 상세에서 사용한 `item.html` 뷰 템플릿을 그대로 재활용한다.

실행해서 상품이 잘 저장되는지 확인하자.

<br>

> 주의!

- 상품 등록과 같은 URL `basic/item` 을 사용한다.
- 같은 URL이 오더라도 HTTP 메서드로 구분하는 것이다.
- 위 코드는 `@PostMapping` 이므로 save 가 호출된다.
- `@GetMapping` 이면 addForm 이 호출 될 것이다!

<br>

## 8.3 addItemV2 - @ModelAttribute

`@RequestParam` 으로 변수를 하나하나 받아서 `Item` 을 생성하는 과정은 불편했다.

이번에는 `@ModelAttribute` 를 사용해서 한번에 처리해보자.

```java
    /**
     * @ModelAttribute("item") Item item
     * model.addAttribute("item", item); 자동 추가
     */
    @PostMapping("/add")
    public String addItemV2(@ModelAttribute("item") Item item, Model model) {
        itemRepository.save(item);
        //model.addAttribute("item", item); //자동 추가, 생략 가능
        return "basic/item";
    }
```

> @ModelAttribute - 요청 파라미터 처리

`@ModelAttribute` 는 `Item` 객체를 생성하고, 요청 파라미터의 값을 프로퍼티 접근법(setXxx)으로 입력해준다.

<br>

> @ModelAttribute - Model 추가

`@ModelAttribute` 는 중요한 한가지 기능이 더 있는데, 바로 모델(Model)에 `@ModelAttribute` 로 지정한 객체를 자동으로 넣어준다.

지금 코드를 보면 `model.addAttribute("item", item)` 가 주석처리 되어 있어도 잘 동작하는 것을 확인할 수 있다.

모델에 데이터를 담을 때는 이름이 필요하다. 이름은 `@ModelAttribute` 에 지정한 `name(value)` 속성을 사용한다.

<br>

> 만약 다음과 같이 `@ModelAttribute` 의 이름을 다르게 지정하면 다른 이름으로 모델에 포함된다.

- `@ModelAttribute("hello") Item item` → 이름을 hello 로 지정
- `model.addAttribute("hello", item);` → 모델에 hello 이름으로 저장

<br>

## 8.3 addItemV3 - ModelAttribute 이름 생략

`@ModelAttribute` 의 이름을 생략할 수 있다.

```java
    @PostMapping("/add")
    public String addItemV3(@ModelAttribute Item item) {
        itemRepository.save(item);
        return "basic/item";
    }
```

- `@ModelAttribute` 의 이름을 생략하면 모델에 저장될 때 클래스명을 사용한다. 이때 클래스의 첫글자만 소문자로 변경해서 등록한다.
- e.g., `@ModelAttribute` 클래스명 모델에 자동 추가되는 이름: Item → item, HelloWorld → helloWorld

<br>

## 8.3 aaddItemV4 - ModelAttribute 전체 생략

`@ModelAttribute` 자체도 생략가능하다. 대상 객체는 모델에 자동 등록된다. 나머지 사항은 기존과 동일하다.

하지만, 강사님은 `ModelAttribute` 전체 생략하는 것을 그리 선호하지는 않는다고 한다.

```java
    /**
     * @ModelAttribute 자체 생략 가능
     * model.addAttribute(item) 자동 추가
     */
    @PostMapping("/add")
    public String addItemV4(Item item) {
        itemRepository.save(item);
        return "basic/item";
    }
```

<br><br>

# 9.상품 수정

## 9.1 컨트롤러

BasicItemController에 추가

```java
    @GetMapping("/{itemId}/edit")
    public String editForm(@PathVariable Long itemId, Model model){
        Item item = itemRepository.findById(itemId);
        model.addAttribute("item", item);
        return "basic/editForm";
    }
```

<br>

## 9.2 뷰 템플릿

- 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하고 다음과 같이 수정하자.
- `/resources/static/html/editForm.html` → 복사 → `/resources/templates/basic/editForm.html`

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
    <style>
      .container {
        max-width: 560px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="py-5 text-center">
        <h2>상품 수정 폼</h2>
      </div>

      <form action="item.html" th:action method="post">
        <div>
          <label for="id">상품 ID</label>
          <input
            type="text"
            id="id"
            name="id"
            class="form-control"
            value="1"
            th:value="${item.id}"
            readonly
          />
        </div>

        <div>
          <label for="itemName">상품명</label>
          <input
            type="text"
            id="itemName"
            name="itemName"
            class="form-control"
            value="상품A"
            th:value="${item.itemName}"
          />
        </div>

        <div>
          <label for="price">가격</label>
          <input
            type="text"
            id="price"
            name="price"
            class="form-control"
            th:value="${item.price}"
          />
        </div>

        <div>
          <label for="quantity">수량</label>
          <input
            type="text"
            id="quantity"
            name="quantity"
            class="form-control"
            th:value="${item.quantity}"
          />
        </div>

        <hr class="my-4" />

        <div class="row">
          <div class="col">
            <button class="w-100 btn btn-primary btn-lg" type="submit">
              저장
            </button>
          </div>

          <div class="col">
            <button
              class="w-100 btn btn-secondary btn-lg"
              onclick="location.href='item.html'"
              th:onclick="|location.href='@{/basic/items/{itemId}(itemId=${item.id})}'|"
              type="button"
            >
              취소
            </button>
          </div>
        </div>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

![](/assets/images/2025/2025-03-11-15-58-28.png)

<br><br>

# 10. 상품 수정 처리

```java
    @PostMapping("/{itemId}/edit")
    public String editItem(@PathVariable Long itemId, Item item){
        itemRepository.update(itemId, item);
        return "redirect:/basic/items/{itemId}";
    }
```

- 상품 수정은 상품 등록과 전체 프로세스가 유사하다.
  - `GET /items/{itemId}/edit` : 상품 수정 폼
  - `POST /items/{itemId}/edit` : 상품 수정 처리

<br>

> 리다이렉트

- 상품 수정은 마지막에 뷰 템플릿을 호출하는 대신에 상품 상세 화면으로 이동하도록 리다이렉트를 호출한다.
- 스프링은 `redirect:/...` 으로 편리하게 리다이렉트를 지원한다.
- `redirect:/basic/items/{itemId}`
  - 컨트롤러에 매핑된 `@PathVariable` 의 값은 `redirect` 에도 사용 할 수 있다.
  - `redirect:/basic/items/{itemId}` → `{itemId}` 는 `@PathVariable Long itemId` 의 값을 그대로 사용한다.

<br>

> 📌 HTML Form 전송은 PUT, PATCH를 지원하지 않는다. GET, POST만 사용할 수 있다.

- PUT, PATCH는 HTTP API 전송시에 사용
- 스프링에서 HTTP POST로 Form 요청할 때 히든 필드를 통해서 PUT, PATCH 매핑을 사용하는 방법이 있지만, HTTP 요청상 POST 요청이다.

<br><br>

# 11. PRG Post/Redirect/Get

> 지금까지 진행한 상품 등록 처리 컨트롤러는 심각한 문제가 있다. (addItemV1 ~ addItemV4)

- 상품 등록을 완료하고 웹 브라우저의 새로고침 버튼을 클릭해보자.
- 상품이 계속해서 중복 등록되는 것을 확인할 수 있다.<br><br>
  ![](/assets/images/2025/2025-03-11-15-55-15.png)

<br>

![](/assets/images/2025/2025-03-11-15-46-17.png)

<br>

> 그 이유는 다음 그림을 통해서 확인할 수 있다.

![](/assets/images/2025/2025-03-11-15-59-21.png)

웹 브라우저의 새로 고침은 마지막에 서버에 전송한 데이터를 다시 전송한다.

1. 상품 등록 폼에서 데이터를 입력하고 저장을 선택하면 `POST /add` + 상품 데이터를 서버로 전송한다.
2. 이 상태에서 새로 고침을 또 선택하면 마지막에 전송한 `POST /add` + 상품 데이터를 서버로 다시 전송하게 된다.

<br>

그래서 내용은 같고, ID만 다른 상품 데이터가 계속 쌓이게 된다.

```
이 문제를 어떻게 해결할 수 있을까? 다음 그림을 보자.
```

<br>

> 웹 브라우저의 새로 고침은 마지막에 서버에 전송한 데이터를 다시 전송한다.

![](/assets/images/2025/2025-03-11-16-00-19.png)

- 새로 고침 문제를 해결하려면 상품 저장 후에 뷰 템플릿으로 이동하는 것이 아니라, 상품 상세 화면으로 리다이렉트를 호출해주면 된다.
- 웹 브라우저는 리다이렉트의 영향으로 상품 저장 후에 실제 상품 상세 화면으로 다시 이동한다.
- 따라서 마지막에 호출한 내용이 상품 상세 화면인 `GET /items/{id}` 가 되는 것이다.
- 이후 새로고침을 해도 상품 상세 화면으로 이동하게 되므로 새로 고침 문제를 해결할 수 있다.

<br>

> BasicItemController에 추가

상품 등록 처리 이후에 뷰 템플릿이 아니라 상품 상세 화면으로 리다이렉트 하도록 코드를 작성해보자.

```java
		/**
		 * PRG - Post/Redirect/Get
		 */
    @PostMapping("/add")
    public String addItemV5(Item item) {
        itemRepository.save(item);
        return "redirect:/basic/items/" + item.getId();
    }
```

- 이런 문제 해결 방식을 `PRG Post/Redirect/Get` 라 한다.
- 위 addItemV5를 실행하면 `/basic/items/add` -> `/basic/items/4` 로 상세 화면으로 리다이렉트 된 것을 확인할 수 있다!

<br>

> ⚠️ `"redirect:/basic/items/" + item.getId() redirect`에서 `+item.getId()` 처럼 URL에 변수를 더해서 사용하는 것은 URL 인코딩이 안되기 때문에 위험하다. 다음에 설명하는 RedirectAttributes 를 사용하자.

<br><br>

# 12. RedirectAttributes

지금까지 상품을 저장하고 상품 상세 화면으로 리다이렉트를 하였다.

저장이 잘 되었으면 상품 상세 화면에 "저장되었습니다"라는 메시지를 보여주자

```java
    @PostMapping("/add")
    public String addItemV6(Item item, RedirectAttributes redirectAttributes) {
        Item savedItem = itemRepository.save(item);

        redirectAttributes.addAttribute("itemId", savedItem.getId());
        redirectAttributes.addAttribute("status", true);
        return "redirect:/basic/items/{itemId}";
    }
```

리다이렉트 할 때 간단히 `status=true` 를 추가해보자. 그리고 뷰 템플릿에서 이 값이 있으면, 저장되었습니다. 라는 메시지를 출력해보자.

상품 등록을 하면 다음과 같은 리다이렉트 결과가 나온다.
[http://localhost:8080/basic/items/3?status=true](http://localhost:8080/basic/items/3?status=true)

<br>

> **RedirectAttributes**

- `RedirectAttributes` 를 사용하면 URL 인코딩도 해주고, `pathVariable` , 쿼리 파라미터까지 처리해준다.
- `redirect:/basic/items/{itemId}`
  - pathVariable 바인딩: `{itemId}`
  - 나머지는 쿼리 파라미터로 처리: `?status=true`

<br>

> **뷰 템플릿 메시지 추가**

`resources/templates/basic/item.html`

```html
<div class="container">
  <div class="py-5 text-center">
    <h2>상품 상세</h2>
  </div>
  <!-- 추가 -->
  <h2 th:if="${param.status}" th:text="'저장 완료!'"></h2>
</div>
```

- `th:if` : 해당 조건이 참이면 실행
- `${param.status}` : 타임리프에서 쿼리 파라미터를 편리하게 조회하는 기능 - 원래는 컨트롤러에서 모델에 직접 담고 값을 꺼내야 한다. 그런데 쿼리 파라미터는 자주 사용해서 타임리프에서 직접 지원한다.

<br>

뷰 템플릿에 메시지를 추가하고 실행해보면 "저장 완료!" 라는 메시지가 나오는 것을 확인할 수 있다.

물론 상품 목록에서 상품 상세로 이동한 경우에는 해당 메시지가 출력되지 않는다.

![](/assets/images/2025/2025-03-11-16-22-26.png)

<br>
