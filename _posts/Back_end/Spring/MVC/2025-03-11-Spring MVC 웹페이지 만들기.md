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

그 후 `build.gradle` 열어주기

![](/assets/images/2025/2025-03-11-09-11-54.png)

<br>

Settings > Annotation Processors에서 저 체크표시를 켜줘야 롬복이 적용된다.

![](/assets/images/2025/2025-03-11-09-12-44.png)

<br>

Settings > Gradle 에서 위 두 개 IntelliJ IDEA로 설정하면 조금 더 빨리 실행할 수 있다.

![](/assets/images/2025/2025-03-11-09-14-07.png)

<br>

## 1.2 Welcome 페이지 추가

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

<br><br>

# 5. 상품 목록 - 타임리프

본격적으로 컨트롤러와 뷰 템플릿을 개발해보자.

<br>

## 5.1 BasicItemController

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
        itemRepository.save(new Item("testA", 10000, 10));
        itemRepository.save(new Item("testB", 20000, 20));
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

다양한 방법이 있지만, 생성자 주입이 가장 권장됨

```java
@RequiredArgsConstructor
public class BasicItemController {
    private final ItemRepository itemRepository;
}
```

<br>

### 5.1.2 @RequiredArgsConstructor

> `final` 이 붙은 멤버변수만 사용해서 생성자를 자동으로 만들어준다.

```java
public BasicItemController(ItemRepository itemRepository) {
this.itemRepository = itemRepository;
}
```

- 이렇게 생성자가 딱 1개만 있으면 스프링이 해당 생성자에 `@Autowired` 로 의존관계를 주입해준다.
- 따라서 final 키워드를 빼면 안된다!, 그러면 `ItemRepository` 의존관계 주입이 안된다.
- 스프링 핵심원리 - 기본편 강의 참고

<br>

> 아래와 같이 생성자 위에 `@Autowired` 를 사용하여 의존성 주입을 해도 된다.

```java
    @Autowired
    public BasicItemController(ItemRepository itemRepository) {
        this.itemRepository = itemRepository;
    }
```

<br><br>

## 5.2 뷰 템플릿

> items.html 정적 HTML을 뷰 템플릿(templates) 영역으로 복사하고 다음과 같이 수정하자 (동적으로 만들기 위함)

- `/resources/static/html/items.html` → 복사 → `/resources/templates/basic/items.html`
- 아래는 타임리프를 사용하였다. 타임리프에 대한 자세한 내용은 [타임리프(Thymeleaf)] 포스팅을 참고하자.

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

<br>

<br>

<br>

<br>

<br>

<br><br>

<br>
