---
title: "[C] 메모리 동적 할당(malloc, free)"
categories: [C]
tag: [C]
toc: true
toc_label: Contents
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# ▶ 메모리 구조

![](https://velog.velcdn.com/images/sieunpark/post/f87a3624-4871-463c-8b59-6813b66e3510/image.png)

- stack
  - 데이터를 일시적으로 저장
  - ex) 함수 매개변수, 지역변수, 반환주소 등
  - 블록 스코프`{}` 내에서만 데이터 저장
- heap⭐
  - <span style="color:CornflowerBlue">프로그래머</span>가 직접 할당할 수 있는 저장공간 (<span style="color:indianred">메모리 동적 할당 가능</span>)
  - 메모리로 할당했다면 반환해야 함. 반환하지 않는다면 메모리 누수(memory leak)가 발생하기 때문
  - ex) C의 Malloc(), 모든 자바 객체 (new 연산자로 생성)
- data
  - 프로그램이 실행되는 동안 유지할 데이터 저장
  - ex) 전역변수, static 변수
- text (code)
  - 실행할 수 있는 코드, 기계어로 이뤄진 명령어가 저장
  - 읽기 전용 공간

<br>

---

<br>

# ▶ 메모리 할당

## ▷ 정적 메모리 할당

- 정적 메모리 할당은 <span style="color:indianred">컴파일</span>을 할 때 메모리 크기를 결정한다.
- 즉, 프로그램 컴파일 시 <span style="color:indianred">stack</span>영역에 얼마만큼의 크기로 할당을 해야하는지 결정되기 때문에, 컴파일 이후 크기가 변경될 수 없다.
- 따라서 정적 배열 선언 시 크기를 <span style="color:CornflowerBlue">가변적</span>으로 명시하면 문제가 될 수 있으므로 반드시 <span style="color:CornflowerBlue">상수</span>로 명시해야 한다.

<br>

- 아래 코드를 살펴보자!<span style="color:CornflowerBlue">

```jsx
int main()
{
  	int n = 10;

    // n의 값이 변수이면 사이즈를 알 수 없기 때문에 에러 발생(정적할당)
    int arr[n];

    // int arr[5]라면, 4byte x 5 = 20byte가 할당이 되므로 정상 출력
    int arr[5];
}
```

<br>

## ▷ 동적 메모리 할당

- 반면, <span style="color:indianred">실행 중</span>에 메모리를 할당하고 싶을 때 <span style="color:indianred">메모리 동적 할당</span>을 사용한다.
- 동적으로 메모리를 할당하는 경우 데이터가<span style="color:indianred"> heap</span> 영역에 할당되어 프로그래머가 직접 할당할 수 있다.

<br>

- malloc()
  - malloc 함수는 인자로 전달된 크기 만큼의 메모리를 동적으로 할당 한 후에, 그 메모리의 시작 주소값을 반환한다.
  - `malloc(할당하고 싶은 크기(byte));`
  - malloc을 사용하기 위해서 <stdlib.h> 헤더파일을 include 해야한다. (`#include <stdlib.h>
`)
    ![](https://velog.velcdn.com/images/sieunpark/post/7b13982f-8382-4e18-9ea3-f6b9ee4c5b67/image.jpg)

<br>

> 동적 메모리 선언 방법

```jsx
int *p;
p = (int *)malloc(sizeof(int)); //동적 메모리 할당
*p = 1000; //동적 메모리 사용
free(p); //동적 메모리 반납
```

<br>

> 동적 메모리 할당 예시

```jsx
#include <stdio.h>
#include <stdlib.h>

void main()
{
    int* arr;
    arr = (int*)malloc(sizeof(int) * 4); // size 4 동적 할당

    arr[0] = 100;
    arr[1] = 200;
    arr[2] = 300;
    arr[3] = 400;

    for (int i = 0; i < 4; i++) {
        printf("arr[%d] : %d\n", i, arr[i]);
    }

    free(arr); //동적할당 해제
}
```

<br>

- 할당된 메모리가 int형의 주소이므로 `int* arr;`을 통해 포인터를 선언해야한다!

- malloc 앞에 반드시 어떤 포인터로 쓸 것인지 형변환을 해줘야 한다.`(int*)`
- sizeof는 byte를 구하는 연산자이다.
  따라서 int형(4byte) n개를 할당하고싶으면, `malloc(n * sizeof(int))`을 하여 n \* 4byte의 메모리가 동적 할당되게 된다.
- ⭐`free(포인터 이름);`을 통해 할당된 메모리는 반납해야한다. 반납하지 않는다면 <span style="color:indianred">메모리 누수(memory leak)</span> 가 발생하기 때문이다.

<br>

- 포인터 변수 arr는 stack에 할당되며, malloc으로 할당된 arr 배열은 heap에 할당된다.
  ![](https://velog.velcdn.com/images/sieunpark/post/8bf27d13-f685-4b52-bc89-82eb20e052c3/image.jpg)

<br>

---

<br>

# ✏️문제

> 정수 10을 저장할 수 있는 동적메모리 할당하고 0부터 9까지 출력하기

```jsx
#include <stdio.h>
#include <stdlib.h>

#define SIZE 10

void main()
{
	// 정적 : int a = 10;

	int* a;
	a = (int*)malloc((sizeof(int) * SIZE));

	for (int i = 0; i < SIZE; i++) {
		a[i] = i;
		printf("%d\n", a[i]);
	}
	free(a);
}
```

<br>

---

<br>

# 📎참조

- https://www.youtube.com/watch?v=uxUDtU5CmDM
- https://suyeon96.tistory.com/26
- https://modoocode.com/243
- https://coding-factory.tistory.com/671
- https://aeunhi99.tistory.com/44
