---
title: "[Java] 사용자 입력(BufferedReader와 Scanner)"
categories: [Java]
tag: [Java]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> 사용자 입력을 받는 방식은 두 가지가 있다.

1. Scanner
2. BufferedReader

<br>

# 1. Scanner

## 1.1 정의

자바의 기본 입력 클래스로, java.util 패키지 내에 존재한다.

<br>

## 1.2 특징

- 입력 받은 즉시 자료형이 확정된다.
- 사용하기 편하지만 속도가 느리다.
- System.in을 생성할 때 내부적으로 try-catch를 사용한다.
- 공백문자(공백, 개행, 탭 등)를 이용하여 토큰을 분리하고 각각의 토큰을 자료형에 따라 변환한다.
  ("a", 3.5, "hello")와 같이 토큰 분리
  - BufferReader은 토큰으로 쪼개지 않고, 입력받은 값 전체를 문자열로 변환한다.

![](/assets/images/2024/2024-03-21-12-39-43.png)

<br>

## 1.3 주요 메소드

- 숫자 입력 : nextX()
  - 이 때, X의 자리에는 입력받을 데이터 타입(Byte, Short, Int, Long, Float, Double)을 적어준다.
  - (ex) `nextInt()`, `nextDouble()`<br>
- 문자열 입력 : `next()`, `nextLine()`
  - `next()` : 스페이스 즉, 공백 전까지 입력받은 문자열을 반환
  - `nextLine()` : Enter를 치기 전까지 쓴 문자열을 모두 반환

|      메소드       |                 설명                  |
| :---------------: | :-----------------------------------: |
|   void close()    |           스캐너를 닫는다.            |
| boolean hasNext() | 입력으로 다른 토큰이 있으면 true 반환 |

```java
// next() vs nextLine()

import java.util.Scanner;

public class _05_practice {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);

		System.out.println("첫 번째 정수 입력 : ");
		int i = scanner.nextInt();
		System.out.println(i);

		System.out.println("첫 번째 문자열 입력 : ");
		String str1 = scanner.nextLine();
		System.out.println(str1);

		System.out.println("두 번째 정수 입력 : ");
		int j = scanner.nextInt();
		System.out.println(j);

		System.out.println("두 번째 문자열 입력 : ");
		String str2 = scanner.next();
		System.out.println(str2);

	}
}
```

- nextInt() 메소드 다음에 nextLine() 메소드를 실행하려고 할때 nextLine()메소드가 그냥 넘어가버리는 오류가 생겨난다.
- 이 이유는 nextInt()메소드를 실행 할 때 20을 콘솔에 입력하고 엔터를 누를때 20을 리턴시켰지만 Enter값은 그대로 남아있다.
- nextLine() 메소드는 Enter값을 기준으로 메소드를 종료시키기 때문에 nextLine()메소드가 실행될 때 남아있는 Enter값을 그대로 읽어 바로 종료된 것이다.
- 그래서 첫번째 문자열입력: 이 넘어가고, 두번째 정수입력: 이 출력된 것이다.만약 정수를  입력하고 그다음 문자를 입력하려고 할 때 next() 메소드를 사용하여야 한다.  아니면 위에 nextLine() 메소드를 한번더 써줘서 enter값을 없애줘야한다.
- 출처: https://deftkang.tistory.com/55 [deftkang의 IT 블로그:티스토리]

<br>

## 1.4 사용 방법

> 기본형

```java
Scanner scanner = new Scanner(System.in);
```

```java
import java.io.*;
//(1) Scanner 호출
import java.util.Scanner;

public class _Quiz_06 {
	public static void main(String[] args) {
		// (2) System.in을 통해 Scanner 객체 생성
		Scanner scanner = new Scanner(System.in);

		// (3) 입력 받을 변수를 자료형에 맞춰 선언
		System.out.print("이름을 입력하세요 : ");
		String inString = scanner.nextLine();
		System.out.println("이름 : " + inString);

		// 버퍼 비우기
		scanner.nextLine();

		System.out.println("정수형과 실수 형 숫자 2개를 입력하세요");
		int numInt = scanner.nextInt();
		float numFloat = scanner.nextFloat();
		float sum = numInt + numFloat;
		System.out.println("합 : " + sum);

		// (4) Scanner 사용 후 꼭 닫아주어야 함 -> 리소스 노출 방지
		scanner.close();
	}
}
```

![](/assets/images/2024/2024-04-10-10-31-06.png)

<br>

- `System.in` 이란?
  - 사용자로부터 입력을 받기 위한 입력 스트림
  - Scanner 클래스뿐 아니라 다른 입력 클래스들도 System.in을 통해 사용자 입력을 받아야 한다.

<br><br>

# 2. BufferedReader

## 2.1 정의

버퍼를 이용하는 대표적인 I/O 클래스이다.

<br>

## 2.2 특징

- 입력받은 값은 String 타입이므로 필요에 따라 타입변환을 해야한다.
- 입력된 데이터를 바로 전달하는 것이 아닌, 버퍼에 저장한 후 전달한다.
- 많은 양의 데이터를 입력받을 경우 성능적인 면에서 효율적이다.

<br>

## 2.3 용어

- Buffer : 데이터를 전송할 때, 일시적으로 그 데이터를 보관하는 임시메모리 영역, 입출력 속도 향상을 위해 사용된다.
- Buffer flush : 버퍼에 남아 있는 데이터를 출력시킨다.(버퍼를 비운다)
- BufferedReader : 버퍼를 이용한 입력
- BufferedWriter : 버퍼를 이용한 출력

<br>

## 2.4 주요 메소드

입력받은 값은 String이므로 타입 변환 필요

| 메소드                                | 설명               |
| ------------------------------------- | ------------------ |
| `inbr.readLine()`                     | 문자열 입력 메소드 |
| `Integer.parseInt(inbr.readLine())`   | 정수 입력 메소드   |
| `Double.parseDouble(inbr.readLine())` | 실수 입력 메소드   |

<br>

## 2.5 사용 방법

- BufferedReader는 String(문자열)만 입력 받을 수 있다.
- 따라서 int값을 할당받고 싶다면, `int num = Integer.parseInt(reader.readLine());` 처럼 형 변환을 해줘야 한다.

> 기본형

```java
BufferedReader inbr = new BufferedReader(new InputStreamReader(System.in));
```

![](/assets/images/2024/2024-04-10-11-38-57.png)

-> 위 코드는 키보드로부터 사용자 입력을 받을 수 있는 BufferReader 객체를 생성하는 것.<br>
-> 이제 BufferReader 객체인 reader을 사용하여 입력을 읽을 수 있다.

<br>

```java
// (1) 3가지 import
// import java.io.*; 한번에 import를 처리해줘도 된다.
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException; // 에외 처리를 위함

public class useInput {
	public static void main(String[] args) throws IOException {
		// (2) 매개변수로 InputStreamReader를 사용하여 객체 생성
		BufferedReader inbr = new BufferedReader(new InputStreamReader(System.in));

		// (3) 입력 받을 변수를 자료형에 맞춰 선언
		try {
			System.out.println("숫자를 입력하세요.");
			int num = Integer.parseInt(inbr.readLine());
			System.out.println("숫자: " + num);

			System.out.println("문자를 입력하세요.");
			String str = inbr.readLine();
			System.out.println("문자: " + str);

			// (4) BufferedReader 사용 후 꼭 닫아주어야 함
			inbr.close();
		} catch (IOException e) {
			System.err.println("입력 오류: " + e.getMessage());
		}
	}
}
```

<br><br>

# 3. Scanner vs BufferReader

> `Scanner`와 `BufferReader` 클래스는 둘 다 사용자(키보드) 입력을 받을 수 있는 기능을 제공한다. 하지만, 두 클래스의 가장 큰 차이는 속도에 있다.

| 기준              | BufferedReader                                                           | Scanner                                                               |
| ----------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **속도**          | 빠름 (버퍼 사이즈가 크기 때문)                                           | 상대적으로 느림 (정규 표현식 적용, 입력 값 분할, 파싱 등의 과정 때문) |
| **문자열 파싱**   | 단순히 읽어들임, 문자열 입력만 직접 처리하고 숫자 등 다른 타입 변환 필요 | 문자열 파싱 가능, 다양한 타입의 입력 처리가 쉬움                      |
| **데이터 처리량** | 대량의 데이터 처리에 적합                                                | 간단한 입력 작업에 적합                                               |
| **버퍼 사이즈**   | 8192byte(8KB)                                                            | 1024byte(1KB)                                                         |
| **동기화**        | O                                                                        | X                                                                     |

![](/assets/images/2024/2024-04-09-13-20-45.png)

<br><br>

# 4. 퀴즈

## 4.1 퀴즈 #1

> 문제

Scanner과 BufferedReader을 사용하여 아래와 같이 출력하세요.

![](/assets/images/2024/2024-04-10-14-10-20.png)

<br>

> 정답

Scanner 사용

```java
import java.util.Scanner;

public class _Quiz_06 {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);

		System.out.println("정수와 실수를 입력하세요! : ");
		int intNum=scanner.nextInt();
		double dblNum=scanner.nextDouble();
		System.out.println("입력한 정수는 "+ intNum + "입니다.");
		System.out.println("입력한 실수는 "+ dblNum + "입니다.");

		scanner.close();
	}
}
```

<br>

BufferedReader 사용

```java
import java.io.*;

public class _Quiz_06 {
	public static void main(String[] args) throws Exception{
		BufferedReader inbr = new BufferedReader(new InputStreamReader(System.in));

		System.out.println("정수와 실수를 입력하세요! : ");
		int intNum=Integer.parseInt(inbr.readLine());
		double dblNum=Double.parseDouble(inbr.readLine());

		System.out.println("입력한 정수는 "+ intNum + "입니다."); // 문자열을 정수로 변환
		System.out.println("입력한 실수는 "+ dblNum + "입니다."); // 문자열을 실수로 변환

		inbr.close();
	}
}
```

<br>

## 4.2 퀴즈 #2

> 문제

- 키보드로부터 원의 반지름을 정수로 입력 받는다.
- 원의 원둘레(원주)를 구하고, 반지름과 원주를 출력하는 프로그램을 작성하시오
- 처리조건
  - 파이(π = 3.141592)는 double형 변수에 저장한다.
  - 원주 공식은 2πr이다. (r은 반지름)

![](/assets/images/2024/2024-04-10-14-49-41.png)

<br>

> 정답

내 풀이

```java
import java.util.Scanner;

public class _Quiz_06 {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		double pi = 3.141592;

		System.out.print("원의 반지름을 정수로 입력하세요 : ");
		int radius = scanner.nextInt();
		System.out.println("원의 반지름 : " + radius);

		double circumference = 2 * pi * radius;
		System.out.println("원의 원둘레 : " + circumference);

		scanner.close();
	}
}
```

<br>

공식 풀이(Math 클래스 사용)

```java
import java.util.Scanner;
import java.lang.Math; // Math 클래스의 파이(PI) 사용

public class _Quiz_06 {
	public static void main(String[] args)  {
		Scanner scanner = new Scanner(System.in);
		double pi = 3.141592;

		System.out.print("원의 반지름을 정수로 입력하세요 : ");
		int radius = scanner.nextInt();
		System.out.println("원의 반지름 : " + radius);

		double circumference = (2 * Math.PI * radius);
		System.out.println("원의 원둘레 : " + circumference);

		scanner.close();
	}
}
```

<br>

## 4.3 퀴즈 #3

> 문제

스포츠용품 매장에서 운동복을 세일 가격으로 구입하였다.<br>
상품의 가격(정수, ex-50000)과 할인율(정수,ex-20)을 키보드로부터 입력을 받아서 구입가격(세일 가격)을 계산하는 프로그램을 작성하시오. (출력 항목: 상품 가격, 할인율, 세일 가격)

![](/assets/images/2024/2024-04-10-16-24-03.png)

<br>

> 정답

Scanner 사용

```java
import java.util.Scanner;

public class _Quiz_ {
	public static void main(String[] args)  {
		Scanner scanner = new Scanner(System.in);

		System.out.print("상품 가격을 입력하세요 : ");
		int price = scanner.nextInt();

		System.out.print("할인율을 입력하세요(%) : ");
		int discountRate = scanner.nextInt();

		int salePrice =  price*(100-discountRate)/100;

		System.out.println("상품 가격 : " + price);
		System.out.println("할인율 : " + discountRate + "%");
		System.out.println("세일 가격 : " + salePrice);
	}
}
```

<br>

BufferedReader 사용

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class _Quiz_ {
	public static void main(String[] args) throws Exception {
		BufferedReader inbr = new BufferedReader(new InputStreamReader(System.in));

		System.out.print("상품 가격을 입력하세요 : ");
		int price = Integer.parseInt(inbr.readLine());

		System.out.print("할인율을 입력하세요(%) : ");
		int rate = Integer.parseInt(inbr.readLine());

		int salePrice = price * (100 - rate) / 100;

		System.out.println("상품 가격 : " + price);
		System.out.println("할인율 : " + rate+"%");
		System.out.println("세일가격 : " + salePrice);
	}
}
```

<br><br>

# 참조

- [자바(JAVA) - Scanner & BufferedReader](https://dlee0129.tistory.com/238)
- [[JAVA] Scanner와 BufferedReader](https://velog.io/@hiy7030/JAVA-Scanner%EC%99%80-BufferedReader)
- [JAVA. 시스템 입출력 : BufferedReader 와 Scanner 비교](https://rang22.tistory.com/37)

<br>
