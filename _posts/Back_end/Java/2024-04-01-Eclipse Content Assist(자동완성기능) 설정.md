---
title: "[Java] Eclipse Content Assist(자동완성기능) 설정"
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

자동 완성 기능이 불편해서 이제는 안 쓴다..

<br>

# 1. 설정하기

eclipse에서 자동완성을 하고자 할 때 `Ctrl+Space` 을 사용하지 않고 따로 설정을 통해 똑같은 기능을 구현해보자

<br>

① eclipse -> window -> Preferences

![](/assets/images/2024/2024-04-01-19-15-10.png)

<br>

② Java -> editor -> content Assist

![](/assets/images/2024/2024-04-01-19-17-46.png)

<br>

③ Auto activation triggers for java 입력란에 아래 문자열 코드를 전체 복사해서 붙여놓기

`<=$:{.@qwertyuioplkjhgfdsazxcvbnm_QWERTYUIOPLKJHGFDSAZXCVBNM`

![](/assets/images/2024/2024-04-01-19-19-13.png)

<br>

이제 `Ctrl+Space`을 하지 않아도 자동완성 기능을 사용할 수 있다!

![](/assets/images/2024/2024-04-01-19-21-29.png)

<br><br>

# 2. 단축키

- `main` -> `public static void main(String[] args) {}`
- `sout` 또는 `sysout` -> `System.out.println();`

<br><br>

# 참조

- [https://devlimk1.tistory.com/9 [개발자의 타임스탬프(TIMESTAMP):티스토리]](https://devlimk1.tistory.com/9)

<br>
