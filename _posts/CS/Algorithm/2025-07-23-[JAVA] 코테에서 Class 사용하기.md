---
title: "[Algorithm/Java] 코테에서 Class 사용하기"
categories: [Algorithm]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 왜 클래스를 쓰는가?

> 여러 데이터를 묶어서 관리할 때 유용

- e.g., 학생 번호 + 점수, 좌표 x/y, 노드 번호 + 가중치 등
- 가독성 향상, 정렬/우선순위 큐에서 비교 기준 만들 때 매우 편리

<br>

> 만약 클래스를 쓰지 않고 코딩하면, 이런 식으로 3개의 배열을 따로 둬야 한다.

```java
int[] nums = new int[N]; // 번호
int[] recommends = new int[N]; // 추천수
int[] times = new int[N]; // 시간
```

이렇게 여러 배열을 각각 관리하면

추천 수가 가장 적고, 오래된 학생을 찾기 위해 세 배열을 동기화해서 탐색해야 하기 때문에, 리스트 정렬도 어렵고 비효율적이다.

<br><br>

# 2. 기본 구조

## 2.1 클래스 기본 구조

클래스를 사용하면?

이렇게 만들면 `List<Student>`로 관리하면서 `sort()`도 편하게 쓸 수 있고, 특정 학생을 찾거나 수정하기도 쉬워진다.

```java
class Student {
    int num; // 학생 번호
    int recommend; // 추천 수
    int time; // 게시된 시간

    Student(int num, int recommend, int time) {
        this.num = num;
        this.recommend = recommend;
        this.time = time;
    }
}
```

<br>

## 2.2 리스트 구조

> `List<Student>`란, Student 객체를 담는 리스트를 의미한다.

리스트에는 Student 객체들이 들어 있다. 예를 들어, 아래와 같은 코드가 있다고 가정하면,

```java
List<Student> list = new ArrayList<>();
frame.add(new Student(2, 1, 0));
frame.add(new Student(5, 3, 1));
```

<br>

위 상태에서 list은 이렇게 생긴 구조이다.

```java
list = [
    Student(num=2, recommend=1, time=0),
    Student(num=5, recommend=3, time=1)
]
```

즉, list 리스트의 각 요소는 Student라는 객체이고,
그 객체는 내부에 다음과 같은 변수(필드) 들을 가지고 있다.

| 필드명      | 값 (예시) |
| ----------- | --------- |
| `num`       | 2         |
| `recommend` | 1         |
| `time`      | 0         |

<br>

> 그래서 `s.num`, `s.recommend` 이런 식으로 접근이 가능한 것이다.

```java
for (Student s : list) {
    System.out.println(s.num); // 학생 번호 출력
    System.out.println(s.recommend); // 추천 수 출력
    System.out.println(s.time); // 게시된 시간 출력
}
```

<br><br>

# 3. 리스트에 담고 정렬하기

정렬도 매우 간단하다.

```java
List<Student> list = new ArrayList<>();

list.sort((a, b) -> {
    if (a.recommend == b.recommend) return a.time - b.time; // 시간 내림차순 정렬
    return a.recommend - b.recommend; // 추천수 내림차순 정렬
});
```

`Collections.sort()`나 `list.sort()`를 사용하여 조건에 따라 정렬

<br><br>

# 4. 우선순위 큐에 담아 쓰기

```java
PriorityQueue<Student> pq = new PriorityQueue<>((a, b) -> {
    if (a.recommend == b.recommend) return a.time - b.time; // 시간 내림차순
    return b.recommend - a.recommend; // 추천수 내림차순
});

pq.offer(new Student(1, 2, 5)); // num, recommend, time
pq.offer(new Student(2, 2, 3)); // 더 빠른 시간

Student top = pq.poll(); // 추천수 가장 높고, 같으면 시간 빠른 학생
```

`PriorityQueue`는 정렬 기준에 따라 자동 정렬되며, 항상 최우선 객체를 `poll()`로 꺼낼 수 있다.

<br><br>

# 5. 컬렉션(Map, Set)과 함께 쓰기

**Map**

```java
Map<Integer, Student> map = new HashMap<>();
map.put(1, new Student(1, 80));
Student s = map.get(1);
```

<br>

**Set**

```java
Set<Student> set = new HashSet<>();
set.add(new Student(1, 90));
set.add(new Student(1, 90)); // 중복? -> 주소 다르면 다른 객체로 간주됨

```

Set에서 중복을 정확히 제거하려면 `equals()`와 `hashCode()` 재정의 필요

<br><br>

# 6. 생성자와 필드 초기화의 원리

**① 생성자 호출 시, 값이 매개변수로 들어옴**

```java
Student s = new Student(3, 5, 10);
```

위 코드에서 (3, 5, 10)은 생성자의 매개변수 num, recommend, time에 각각 전달됨

<br>

**② 생성자 내부에서 필드에 값 저장**

```java
public Student(int num, int recommend, int time) {
    this.num = num; // 필드 num <- 매개변수 num
    this.recommend = recommend;
    this.time = time;
}
```

<br>

**③ 각 필드 변수는 객체 내부에 저장**

| 구분          | 저장 위치             | 수명                              |
| ------------- | --------------------- | --------------------------------- |
| 매개변수      | 스택 메모리           | 생성자 실행 중 잠깐               |
| 필드 변수     | 힙 메모리 (객체 내부) | 객체가 살아있는 동안 계속 유지됨  |
| `this.num` 등 | 힙 메모리 (객체 내부) | 힙에 저장되는 실제 객체의 상태 값 |

- 즉, `num`, `recommend`, `time`은 해당 객체의 **고유한 상태값**이 됨
- 매개변수는 메서드 실행이 끝나면 **사라지지만**, 필드는 객체가 살아있는 동안 계속 유지

<br><br>

# 7. [참고] 코딩 테스트와 실무 객체 필드 관리 비교

## 코딩 테스트용

```java
class Student {
    int num;
    String name;

    Student(int num, String name) {
        this.num = num;
        this.name = name;
    }
}
```

빠르게 객체 생성하고 필드 직접 접근

```java
Student s = new Student(1, "홍길동");
s.name = "이몽룡";               // 필드에 직접 값을 넣음
System.out.println(s.name);     // 직접 가져옴
```

`name`이 `public` 또는 생략(default)이므로 빠르게 객체 생성하고 필드 직접 접근 가능

<br>

## 실무용

```java
public class Student {
    private int num;
    private String name;  // 외부에서 직접 접근 불가

    public Student(int num, String name) {
        this.num = num;
        this.name = name;
    }

    public int getNum() { return num; }
    public String getName() { return name; }

    public void setNum(int num) { this.num = num; }
    public void setName(String name) { this.name = name; }
}
```

- 필드(멤버 변수)는 `private`으로 감추고,
  외부에서는 `getter`와 `setter`를 통해 간접적으로 접근 → **캡슐화**
- 유효성 검사 가능 (`if (x < 0) throw ...` 등)

```java
Student s = new Student(1, "홍길동");
s.setName("홍길동");             // 값을 설정 (setter)
System.out.println(s.getName()); // 값을 가져옴 (getter)
```

- `name`은 **private**이라 외부에서 바로 못 씀
- 대신 `setName()`으로 넣고, `getName()`으로 꺼냄

<br><br>

# 백준 클래스 사용 문제

> 클래스나 객체 지향 개념(필드, 생성자, 메서드, toString() 등)을 활용해서 푼 문제들을 모아두었다.

- [백준 1713번 - 후보 추천하기](https://mynamesieun.github.io/algorithm/JAVA-%EB%B0%B1%EC%A4%80-1713%EB%B2%88-%ED%9B%84%EB%B3%B4-%EC%B6%94%EC%B2%9C%ED%95%98%EA%B8%B0/)
- [백준 9019번 - DSLR](https://mynamesieun.github.io/algorithm/JAVA-%EB%B0%B1%EC%A4%80-9019%EB%B2%88-DSLR/)
- [백준 1991번 - 트리 순회](https://mynamesieun.github.io/algorithm/JAVA-%EB%B0%B1%EC%A4%80-1991%EB%B2%88-%ED%8A%B8%EB%A6%AC-%EC%88%9C%ED%9A%8C/)
- [백준 20546번 - 🐜기적의 매매법🐜](https://mynamesieun.github.io/algorithm/JAVA-%EB%B0%B1%EC%A4%80-20546%EB%B2%88-%EA%B8%B0%EC%A0%81%EC%9D%98-%EB%A7%A4%EB%A7%A4%EB%B2%95/)

<br>
