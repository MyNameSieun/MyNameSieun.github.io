---
title: "[Java] 사용자 입력(bufferedReader와 Scanner)"
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

# 1. Scanner

## 1.1 정의

자바의 기본 입력 클래스로, java.util 패키지 내에 존재한다.

<br>

## 1.2 특징

- 입력 받은 즉시 자료형이 확정된다.
- 사용하기 편하지만 속도가 느리다.
- System.in을 생성할 때 내부적으로 try-catch를 사용한다.
- 공백, 개행, 탭 등을 기준으로 데이터를 입력 받아 사용자가 입력한 데이터를 쉽게 분리하여 처리할 수 있다.

<br>

## 1.3 주요 메서드

> nextX()

- 주로 `nextX()`의 형태를 이루며 X의 자리에는 입력 받을 데이터의 타입을 적어준다.(ex) `nextInt()`, `nextDouble()`)
- 단, String을 입력 받는 메서드는 `nextString()`이 아닌 `next()`와 `nextLine` 이라고 한다.
  - `next()` : 스페이스 즉, 공백 전까지 입력받은 문자열을 반환한다.
  - `nextLine()` : Enter를 치기 전까지 쓴 문자열을 모두 반환한다.

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

![](/assets/images/2024/2024-04-09-13-51-25.png)

<br>

## 1.4 사용 방법

```java
package chap_04;

// (1) Scanner 호출
import java.util.Scanner;

public class useInput {
	public static void main(String[] args) {
		// (2) System.in을 통해 Scanner 객체 생성
		Scanner sc = new Scanner(System.in);

		// (3) 입력 받을 변수를 자료형에 맞춰 선언
		System.out.println("숫자를 입력하세요.");
		int num = sc.nextInt();
		System.out.println("숫자 : " + num);

	    // 버퍼 비우기
        sc.nextLine();

		System.out.println("문자를 입력하세요.");
		String str = sc.nextLine();
		System.out.println("문자 : " + str);

		// (4) Scanner 사용 후 꼭 닫아주어야 함
		sc.close();
	}
}

```

![](/assets/images/2024/2024-04-09-13-36-06.png)

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

## 2.4 주요 메서드

| 메서드                                | 설명                                                                                                   |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `read()`                              | 다음 문자를 읽어서 int 형으로 반환합니다.                                                              |
| `read(char[] cbuf, int off, int len)` | 주어진 char 배열에 데이터를 읽어옵니다.                                                                |
| `readLine()`                          | 한 줄을 읽어서 문자열로 반환합니다.                                                                    |
| `skip(long n)`                        | 지정된 개수만큼 문자를 건너뜁니다.                                                                     |
| `ready()`                             | 입력 스트림이 데이터를 읽을 준비가 되었는지 여부를 반환합니다.                                         |
| `mark(int readAheadLimit)`            | 현재 위치를 표시하여 나중에 reset() 메서드로 돌아갈 수 있도록 합니다.                                  |
| `reset()`                             | mark() 메서드가 호출된 지점으로 돌아갑니다. 단, mark()로 표시한 위치 이후의 데이터만 읽을 수 있습니다. |
| `close()`                             | 스트림을 닫습니다.                                                                                     |

<br>

## 2.5 사용 방법

- BufferedReader는 String(문자열)만 입력 받을 수 있다.
- 따라서 int값을 할당받고 싶다면, `int num = Integer.parseInt(reader.readLine());` 처럼 형 변환을 해줘야 한다.

```java
package chap_04;

// (1) 3가지 import
// import java.io.*; 한번에 import를 처리해줘도 된다.
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException; // 에외 처리를 위함

public class useInput {
	public static void main(String[] args) throws IOException {
		// (2) 매개변수로 InputStreamReader를 사용하여 객체 생성
		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

		// (3) 입력 받을 변수를 자료형에 맞춰 선언
		try {
			System.out.println("숫자를 입력하세요.");
			int num = Integer.parseInt(reader.readLine());
			System.out.println("숫자: " + num);

			System.out.println("문자를 입력하세요.");
			String str = reader.readLine();
			System.out.println("문자: " + str);

			// (4) BufferedReader 사용 후 꼭 닫아주어야 함
			reader.close();
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

# 참조

- [자바(JAVA) - Scanner & BufferedReader](https://dlee0129.tistory.com/238)
- [[JAVA] Scanner와 BufferedReader](https://velog.io/@hiy7030/JAVA-Scanner%EC%99%80-BufferedReader)
- [JAVA. 시스템 입출력 : BufferedReader 와 Scanner 비교](https://rang22.tistory.com/37)

<br>
