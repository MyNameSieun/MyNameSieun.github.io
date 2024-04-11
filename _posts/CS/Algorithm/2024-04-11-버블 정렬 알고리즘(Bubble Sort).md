---
title: "[Algorithm/Java] 버블 정렬 알고리즘(Bubble Sort)"
categories: [Algorithm]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 버블 정렬 알고리즘(Bubble Sort)

## 1.1 개념

- 서로 인접한 두 원소를 비교하여 자리를 교환하며 자료를 정렬하는 알고리즘이다.
- 시간복잡도 : T(n) = O(n^2)

  - 배열의 n-1의 원소를 비교해야 하기 때문<br><br>
    ![](/assets/images/2024/2024-04-11-18-19-08.png)

<br>

> 버블정렬 알고리즘 이름의 유래 🌊💬

정렬 과정에서 원소의 이동이 마치 거품(bubble)이 물 위로 올라오는 것처럼 보인다는 데에서 유래되었다고 한다.

<br>

## 1.2 절차

1. 인접한 두 원소 비교: 배열의 인접한 두 원소를 비교하여, 순서대로 정렬되어 있지 않으면 서로 위치를 교환(swap)한다.
2. 전체 배열 순회: 첫 번째 원소부터 시작하여 마지막-1 번째 원소까지 이 과정을 반복한다. 이때, 각 순회마다 가장 큰 원소(또는 가장 작은 원소)가 배열의 끝으로 이동한다.
3. 반복 수행: 배열의 길이만큼 이 과정을 반복하며, 각 반복마다 순회하는 범위를 하나씩 줄여간다. (이미 정렬된 원소는 다시 확인할 필요가 없기 때문)

<br>

> 왜 이중 for문을 사용할까?

모든 요소를 한 번씩 비교하기 위해서이다.

<br>

> 첫 번째 for문과 두 번쨰 for문의 연관성

- 첫 번째 for문은 전체 정렬 과정의 횟수를 결정한다.
- 두 번째 for문은 실제로 요소들을 비교하고 위치를 바꾸는 작업을 수행하며, 반복 후 비교 횟수를 줄여나간다.

<br><br>

# 2. 버블 정렬 구현

![](/assets/images/2024/2024-04-11-18-08-00.png)

- 배열의 길이가 n일 때, 마지막 요소의 인덱스는 n-1이라는 것에 주의하자!(배열의 인덱스는 0부터 시작)
- arr.length - 1을 사용하는 것은 배열의 마지막 요소에 접근하기 위함이다.

## 2.1 오름차순 정렬

```java
package work;

public class bubbleSort {
	public static void main(String[] args) {
		int[] arr = { 4, 3, 6, 2, 7 };

		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = 0; j < arr.length - i - 1; j++) {
				if (arr[j] > arr[j + 1]) {
					int temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}

		}
		System.out.print("오름차순 정렬 : ");
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]+" ");
		}
	}
}

```

<br>

## 2.2 내림차순 정렬

```java
package work;

public class bubbleSort {
	public static void main(String[] args) {
		int[] arr = { 4, 3, 6, 2, 7 };

		for (int i = 0; i < arr.length - 1; i++) {
			for (int j = 0; j < arr.length - i - 1; j++) {
				if (arr[j] < arr[j + 1]) {
					int temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}

		}
		System.out.print("내림차순 정렬 : ");
		for (int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]+" ");
		}
	}
}
```

<br>
