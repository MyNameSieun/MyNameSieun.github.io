---
title: "[PHP] Window10 XAMPP 설치(Apache, MySql, PHP)"
categories: [PHP]
tag: [PHP]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. XAMPP 설치

Window10 XAMPP 설치(Apache, MySql, PHP)는 [여기](https://teserre.tistory.com/12) 링크를 통해 하고 오자.

<br><br>

# 2. XAMPP 실행

VsCode로 C > xampp > htdocs를 열어 test.php 파일을 생성하자

![](/assets/images/2024/2024-03-25-23-38-47.png)

<br>

그 후, 아래 코드를 입력하자. `phpinfo()`는 설치되어있는 php 설정 사항을 간략히 보여주는 역할을 한다.

```php
<?php
  echo phpinfo()
?>
```

<br>

다음으로 `http://localhost/test.php`로 접속하면 아래와 같이 뜨게 된다.

![](/assets/images/2024/2024-03-25-23-41-25.png)

<br><br>

# 3. php 설정하기

> c > xampp > php > php.ini를 열어 환경 설정을 할 수 있다.

![](/assets/images/2024/2024-03-25-23-45-51.png)

Ctrl + F를 눌러 `short_`로 검색하여 `short_open_tag=Off`를 `short_open_tag=On`으로 변경하자

<br>

그 후, 다시 패널에서 Apache server를 Stop 한 후에 Start 해주어 변경된 php.ini의 내용을 적용하자.

![](/assets/images/2024/2024-03-25-23-48-59.png)

<br>

`short_open_tag`가 On으로 바뀌면 성공!

![](/assets/images/2024/다시%20.png)

<br>

- 설정 변경 전

```php
<?php
  echo phpinfo()
?>
```

- 설정 변경 후 -> 간략하게 코드 작성 가능

```php
<?
  echo phpinfo()
?>
```

<br><br>

# 4. PHP 관련 익스텐션

## 4.1 PHP Intelephense⭐(코드 정렬)

> - PHP에 대한 고급 자동완성과 코드 리팩터링을 지원한다.

- PHP 코드를 입력할 때마다 문법을 자동으로 확인하여 올바른 구문을 사용하는지 검사

![](/assets/images/2024/2024-03-26-00-01-13.png)

<br>

## 4.2 Format HTML in PHP

> PHP와 HTML이 함께 사용된 파일에서 HTML 부분을 선택한 후 마우스 우클릭하여 코드를 깔끔하게 정리해준다.

![](/assets/images/2024/2024-03-25-23-59-43.png)

<br>

## 4.3 PHP IntelliSense

> 코드 작성 중에 PHP 문법 및 함수에 대한 자동 완성 기능을 제공하여 빠르게 코드를 작성할 수 있도록 도와준다.

![](/assets/images/2024/2024-03-26-00-01-52.png)

<br><br>
