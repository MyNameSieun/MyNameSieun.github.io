---
title: "[Java] ItemRepository로 알아보는 Map 사용법"
categories: [Java]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

---

Spring Boot 애플리케이션을 개발하다 보면, 간단한 데이터 저장소를 구현할 때 Map을 사용하는 경우가 있다.

`ItemRepository` 클래스의 예제를 통해 `Map<Long, Item> store = new HashMap<>();` 코드의 의미와 사용 방법을 알아보자.

---

<br>

# 1. Map

## 1.1 Map<Long, Item>

> Map은 키-값(Key-Value) 쌍으로 데이터를 저장하는 자료구조

- `<Long, Item>`은 **제네릭(Generic)** 을 사용하여 Map에 저장할 키와 값의 데이터 타입을 지정한다.
  - Long: 키의 데이터 타입 (ID나 고유 식별자 역할)
  - Item: 값의 데이터 타입 (저장할 실제 데이터 객체)
- put, get, remove, values, clear와 같은 주요 메서드를 통해 데이터를 효과적으로 관리할 수 있다.

<br>

## 1.2 new HashMap<>()

> HashMap은 Map 인터페이스의 **구현체(Implementation)**

- HashMap은 해시 알고리즘을 사용하여 데이터를 저장하고 빠르게 검색할 수 있다.
- `<>` 안에 타입을 생략해도 자동으로 `<Long, Item>`을 추론한다.

<br>

> 즉, `Map<Long, Item> store = new HashMap<>();`의 의미는 아래와 같다.

- `store`라는 이름의 Map 변수를 선언하고, `HashMap`의 새로운 인스턴스를 생성하여 초기화한다.
- 이 store는 Long 타입의 키와 Item 타입의 값을 저장할 수 있는 저장소가 된다.

<br><br>

# 2. 코드 예시

## 2.1 Item - 상품 객체

```java
package hello.itemservice.domain.item;

@Getter @Setter
public class Item {
    private Long id;
    private String itemName;
    private Integer price;
    private Integer quantity;

    public Item() {

    }

    public Item(String itemName, Integer price, Integer quantity) {
        this.itemName = itemName;
        this.price = price;
        this.quantity = quantity;
    }
}
```

<br>

## 2.2 ItemRepository - 상품 저장소

```java
package hello.itemservice.domain.item;

@Repository
public class ItemRepository {

    private static final Map<Long, Item> store = new HashMap<>();
    private static long sequence = 0L;

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

> private static final을 사용하는 이유

- store를 공유 저장소로 사용하기 위해 `static`을 사용

- store 참조를 변경할 수 없도록 보장하기 위해 `final`을 사용
  ```java
  store = new HashMap<>(); // 컴파일 에러 발생!
  ```
- 접근 제어자 `private`을 통해 외부에서 직접적인 접근을 차단하여 안전하게 데이터를 관리할 수 있음

<br><br>

# 3. Map의 주요 메서드 및 사용법

## 3.1 put(Key, Value)

- Map에 데이터를 추가한다.
- 만약 동일한 키가 이미 존재한다면, 기존 값을 덮어쓴다.

```java
store.put(1L, new Item("아이템1", 1000, 10));
store.put(1L, new Item("아이템2", 2000, 20)); // 키가 같으므로 기존 데이터 덮어쓰기
```

<br>

## 3.2 get(Key)

- 키를 이용해 해당 키에 연결된 값을 반환한다.
- 키가 존재하지 않으면 `null`을 반환한다.

```java
Item item = store.get(1L);
System.out.println(item.getItemName()); // "아이템2" 출력
```

<br>

## 3.3 remove(Key)

- 지정된 키에 해당하는 키-값 쌍을 삭제한다.

```java
store.remove(1L);
System.out.println(store.get(1L)); // null 출력
```

<br>

## 3.4 values()

- Map에 저장된 **모든 값(Value)** 를 Collection으로 반환한다.
- `ItemRepository`의 `findAll()` 메서드처럼 List로 변환하여 사용할 수도 있다.

```java
List<Item> itemList = new ArrayList<>(store.values());
itemList.forEach(item -> System.out.println(item.getItemName()));
```

<br>

## 3.5 clear()

- Map에 저장된 모든 데이터를 삭제한다.

```java
store.clear();
System.out.println(store.size()); // 0 출력
```

<br><br>

# 4. Map 사용 시 주의할 점

- HashMap은 null 키와 null 값을 허용하지만, ConcurrentHashMap은 그렇지 않다.
- HashMap은 Thread-safe하지 않으므로 멀티스레드 환경에서는 사용에 주의해야 한다.
- 즉, 실무에서는 ConcurrentHashMap을 사용하자!

<br>
