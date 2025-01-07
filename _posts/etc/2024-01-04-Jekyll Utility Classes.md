---
title: "[etc] Jekyll Utility Classes"
categories: [etc]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

[Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)의 Utility Classes를 정리한 내용입니다.
{: .notice--danger}

# 1. 정렬

## 1.1 문자 정렬

```js
// 왼쪽 정렬
{: .text-left}

//중앙 정렬
{: .text-center}

// 오른쪽 정렬
{: .text-right}
```

<br>

## 1.2 이미지 정렬

```js
// 왼쪽 정렬
![image](링크)
{: .align-left}

//중앙 정렬
![image](링크)
{: .align-center}

// 오른쪽 정렬
![image](링크)
{: .text-right}
```

<br><br>

# 2. Notices

| Notice Type |      Class       |
| :---------: | :--------------: |
|   Default   |     .notice      |
|   Primary   | .notice--primary |
|    Info     |  .notice--info   |
|   Warning   | .notice--warning |
|   Success   | .notice--success |
|   Danger    | .notice--danger  |

```js
사용 예시
{: .notice--danger}
```

Default
{: .notice}

Primary
{: .notice--primary}

Info
{: .notice--info}

Warning
{: .notice--warning}

Success
{: .notice--success}

Danger
{: .notice--danger}

<br>
