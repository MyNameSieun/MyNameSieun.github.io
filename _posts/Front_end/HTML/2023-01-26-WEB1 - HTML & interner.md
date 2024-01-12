---
title: "[HTML] WEB1 - HTML & internet"
categories: [HTML]
tag: [HTML]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

![](/assets/images/2024/2024-01-05-18-38-23.png)
[생활코딩](https://youtu.be/tZooW6PritE) 공부 내용을 바탕으로 정리한 글 입니다.

# ▶ 기획

- 웹 목적: 코딩 수업과 내용 정리정돈

> **어떤 형태의 웹사이트를 만들 것인가?**<br>1. 제일 위쪽에는 수업의 전체 제목을 표시<br>2. 왼쪽에는 수업의 목차 표시. 목차에는 링크가 걸려 있으며, 링크를 클릭하면 링크에 해당하는 콘텐츠가 오른쪽에 표시 <br>3. 오른쪽의 상단엔 제목이 표시되고, 본문에는 제목에 대한 설명 표시
> ![](https://velog.velcdn.com/images/sieunpark/post/bd464c80-0d5e-4277-9e43-4090c5baa170/image.png)

---

# ▶ TAG를 사용하여 텍스트 표시하기

- 모르는 정보를 본인의 힘으로 직접 찾아보는 것이 매우 중요하다.

| Tag        | 설명                                                    |
| ---------- | ------------------------------------------------------- |
| <`h[1-6]`> | 제목, `<h1>`이 가장 크고 `<h6>`이 가장 작음             |
| <`p`>      | <p>줄바꿈, 단락 표시해줌, css로 세밀한 디자인 변경 가능 |
| <`br`>     | 줄바꿈, 닫기태그 없음                                   |
| <`hr`>     | <hr>텍스트 위 또는 아래에 가로줄 표시<hr>               |
| <`strong`> | <strong>텍스트 굵게 표시, 중요한 내용일 때              |
| <`b`>      | <b>텍스트 굵게 표시, 단순히 굵게 표시할 때              |
| <`em`>     | <em>텍스트 기울임, 강조할 때                            |
| <`i`>      | <i>텍스트 기울임, 단순히 기울여 표시할 때               |
| <`u`>      | <u>텍스트 밑줄 표시                                     |
| <`s`>      | <s>텍스트 취소선 표시                                   |

- 태그는 다음과 같이 사용한다.
  > ![](https://velog.velcdn.com/images/sieunpark/post/afb4cca1-a6b3-4534-a821-245bf83327e6/image.jpg)

---

# ▶ 속성을 활용하여 이미지 불러오기

| Tag     | 설명                   |
| ------- | ---------------------- |
| <`img`> | 이미지를 불러오는 태그 |

> 속성 src를 사용하여 원하는 이미지를 불러오기

```
<img src="이미지링크">
```

저작권 없는 이미지 사이트: [unsplash.com](unsplash.com)

> 속성 width를 사용해 이미지 크기 조정하기

```
<img src="이미지링크" width="500" height="600">
<img src="이미지링크" width="100%">
```

---

# ▶ 목차 만들기

| Tag    | 설명                             |
| ------ | -------------------------------- |
| <`li`> | 목록을 만들어 주는 태그          |
| <`ul`> | 목록 구분하기 위한 태그          |
| <`ul`> | 목록 앞에 숫자를 만들어주는 태그 |

> 1.  순서가 있는 목록

```
<ol>
  <li>항목 1</li>
  <li>항목 2</li>
</ol>
```

2. 순서가 없는 목록

```
<ul>
  <li>항목 1</li>
  <li>항목 2</li>
</ul>
```

---

# ▶ 문서의 구조

## ▷ 문서 제목 지정하기

현재 문서의 제목은 파일명으로 되어있다
![](https://velog.velcdn.com/images/sieunpark/post/4c06b720-2aa8-4c95-a173-ad0ebfbd2e84/image.png)

> 문서의 제목을 직접 지정해보자
>
> | Tag       | 설명                           |
> | --------- | ------------------------------ |
> | <`title`> | 웹 문서의 제목을 지정하는 태그 |
>
> ![](https://velog.velcdn.com/images/sieunpark/post/5850cb64-8dc2-4627-b348-c5fdf23c4f20/image.png)

<br>

## ▷ 한글 글자 크랙 오류 수정하기

웹 페이지는 UTF-8로 만들어져 UTF-8로 열지 않으면 한글 글자가 깨질 수 있기 때문에 다음과 같은 코드를 작성해야한다.

```
<meta charset="utf-8">
```

<br>

## ▷ 문서 분류하기

본문과 본문을 설명하는 정보를 각기 다른 태그로 분리해서 정리 정돈한다.

| Tag               | 설명                                                               |
| ----------------- | ------------------------------------------------------------------ |
| <`!doctype html`> | 웹페이지가 HTML로 만들어졌다는 것을 알려주는 태그                  |
| <`html`>          | 문서의 시작과 끝을 나타내는 태그                                   |
| <`head`>          | 문서에 대한 정보를 나타내는 태그                                   |
| <`body`>          | 문서에 관한 내용을 나타내며, 실제로 웹 페이지 화면에 보여지는 태그 |

> ![](https://velog.velcdn.com/images/sieunpark/post/88666117-b2e3-40ce-9a99-b131aa65fac8/image.jpg)

- 메타데이터란 웹 브라우저에 실질적으로 보여지지 않는 HTML 문서에 대한 정보이다.

아래와 같이 문서를 분류할 수 있다

> ```
>
> ```

<!doctype html>
<html>
<head>
  <title>WEB1 - html</title>
  <meta charset="utf-8">
</head>
<body>
<ol>
    <li>HTML</li>
    <li>CSS</li>
    <li>JaveScript</li>
  </ol>
<h1>HTML</h1>
웹 페이지를 구성하는 가장 기초적인 <u>마크업 언어</u>이다.<br>
<p>텍스트, 이미지, 비디오, 링크 등을 어디에 배치할지 구조화하는 뼈대이며, 추후 CSS를 통해 살을 붙여준다.
<strong>Hyper Text Markup Language</strong>의 약자이다.</p>
<img src="이미지링크">
</body>
</html>
```
  
***
# ▶ 링크 걸기
|Tag|설명|
|----------|----------|
|<```a```>|링크를 걸고 싶은 곳에 사용하면 링크가 걸리는 태그|
>속성 src를 사용하여 링크 클릭시 원하는 페이지 열기
```
<a href="페이지링크">Hyper Text Markup Language</a>
```
![](https://velog.velcdn.com/images/sieunpark/post/f5218b98-04c8-4163-aa01-6f0cd08c3b95/image.png)

> 속성 target="\_blank"을 사용하여 링크 클릭시 새 창에서 원하는 페이지 열기

```
<a href="페이지링크" target="_blank">Hyper Text Markup Language</a>
```

> 속성 title를 사용하여 링크가 어떤 내용을 담고 있는지를 툴팁으로 보여주기

```
<a href="페이지링크" target="_blank" title="HTML이란?">Hyper Text Markup Language</a>
```

![](https://velog.velcdn.com/images/sieunpark/post/559cb57d-b620-481d-9b42-7b784c0edb3f/image.png)

---

# ▶ 웹 사이트 완성하기

## ▷ 웹페이지를 대표하는 제목 만들기

![](https://velog.velcdn.com/images/sieunpark/post/41b06689-00bd-4cd5-a8d6-58a2f2fa790a/image.png)

> ```
>
> ```

<h1>WEB</h1>
```
  ![](https://velog.velcdn.com/images/sieunpark/post/ff2ec02c-2567-43c8-b81c-4880d2573c9e/image.png)

## ▷ 링크를 걸어 다른 페이지와 연결시키기

아래 그림은 링크로 연결될 웹페이지의 파일명이다.
![](https://velog.velcdn.com/images/sieunpark/post/b4e8dc2b-c36d-49f8-8cd7-3ee0efc73f83/image.png)

> 1.  링크가 가리킬 파일을 생성한다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/bbdcc707-87ff-4efa-be80-f449d3ae36cb/image.png)

> 2.  링크를 걸고자 하는 텍스트에 태그와 속성을 사용하여 링크를 생성한다.

```
<!doctype html>
<html>
<head>
  <title>WEB1 - html</title>
  <meta charset="utf-8">
</head>
<body>
<h1><a href="index.html">WEB</a></h1>
<ol>
    <li><a href="1.html">HTML</a></li>
    <li><a href="2.html">CSS</a></li>
    <li><a href="3.html">JaveScript</a></li>
  </ol>
<h2>HTML</h2>
웹 페이지를 구성하는 가장 기초적인 <u>마크업 언어</u>이다.<br>
<p>텍스트, 이미지, 비디오, 링크 등을 어디에 배치할지 구조화하는 뼈대이며, 추후 CSS를 통해 살을 붙여준다.
<strong><a href="https://www.websiterating.com/ko/web-hosting/glossary/what-is-html/" target="_blank" title="HTML이란?">Hyper Text Markup Language</a></strong>의 약자이다.</p>
<img src="https://images.unsplash.com/photo-1515879218367-8466d910aaa4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80" width="25%">
</body>
</html>
```

> 3.  각 파일마다 코드 작성 해주면 완성!
>     ![](https://velog.velcdn.com/images/sieunpark/post/66abcdbf-b0d1-41e6-8b3a-f44f15bf2e56/image.png)

---

# ▶ 파일 공유하기

## ▷ 클라이언트와 서버의 관계

- 웹브라우저가 깔린 컴퓨터는 index.html이라는 정보를 요청하고 웹서버가 깔린 컴퓨터는 웹브라우저에게 정보를 전해주며 서로 정보를 주고 받는다.

![](https://velog.velcdn.com/images/sieunpark/post/8aefd695-ef18-462d-be50-d849035c62c9/image.png)

## ▷ 웹호스팅 github pages을 사용하여 파일 공유하기

- 웹서버에 익숙해지면 내가 만든 컨텐츠를 인터넷을 사용할 수 있는 전 세계의 누구나 사용하도록 있도록 할 수 있다.
- 자신의 컴퓨터에 직접 웹서버를 설치하는 방법과 웹호스팅 업체를 이용하여 파일을 공유할 수 있다.

---

# ▶ 웹호스팅을 이용하여 웹서버 운영하기

github(https://github.com)의 pages 기능을 사용하여 웹서버 운영하기

> 1.  new 버튼을 눌러 소스코드를 보관하는 곳인 저장소(repository)를 생성한다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/34e12adf-4dba-470e-8317-e9b2ca349fa0/image.png)

> 2.  Repository name 에는 프로젝트의 이름을 적고 public으로 설정하여 누구나 자신의 소스코드를 볼 수 있게 해준뒤, 저장소 생성하기(Create repository) 버튼을 눌러준다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/7dbb20a3-ac4d-4f94-8b52-50295d53917e/image.png)

> 3.  아래와 같은 화면이 나타나면 ploading an existing file라는 부분을 클릭해준다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/fa1b765d-dfce-46df-86f5-69d735645fcb/image.png)

> 4.  작업한 파일을 드래그 앤 드롭 한다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/33cbea26-3612-4724-9706-0da7f4965ccc/image.png)

> 5.  파일이 변경될 때마다 업로드를 해야 하기 때문에 변경된 내용을 적는다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/32565672-e851-4e06-b1fb-908dbc23e87f/image.png)

> 6.  Commit changes 버튼을 눌러 업로드를 한다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/c2def0e0-2cf7-4b2b-a6ce-5a89a8d6584c/image.png)
>     업로드가 끝나면 아래와 같이 파일의 목록이 보인다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/7e978d1f-d507-4eb4-9c7e-4714a5740d47/image.png)

> 7.  업로드한 웹페이지를 인터넷을 통해서 서비스 하기 위해 Settings 버튼을 선택한다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/bcb2c349-3d69-486f-a91e-6ef4c8ed8d83/image.png)

> 8.  사이드 바에서 Pages를 선택한 후 Branch를 main 혹은 master를 선택하고 Save 버튼을 누른다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/cfd1c50b-2e66-45da-b863-11c68d10b1da/image.png)

> 9.  Actions를 클릭하여 진행 사항을 확인할 수 있다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/a17fdb16-4d77-4a40-aedd-448a95b5b5fd/image.png)

> 10. 이제 누구나 자신이 만든 웹 사이트에 방문할 수 있다!
>     https://mynamesieun.github.io/WEB1/

> 11. 파일의 내용을 변경하였으면 Add flie에 Upload flies를 클릭하여 본인이 작업한 파일을 다시 드래그하여 변경된 내용을 코멘트를 적은 뒤에 재 업로드를 해주면 된다.
>     ![](https://velog.velcdn.com/images/sieunpark/post/e1b8ffb0-3a93-41c4-a082-9d6049b5ae1f/image.png)

---

# ▶ 부록

## ▷ 동영상 삽입

| Tag       | 설명                                                                                   |
| --------- | -------------------------------------------------------------------------------------- |
| <`ifram`> | 웹페이지 안에 또 다른 HTML페이지를 삽입할 수 있는 태그, 뮤비 등 동영상도 넣을 수 있다. |

> 1.  삽입하고 싶은 동영상 공유하기 버튼 누른 후 퍼가기 클릭
>     ![](https://velog.velcdn.com/images/sieunpark/post/c130d4d6-5ae1-4ab9-86c3-8cdb18d169ac/image.png)
>     <br>2. 소스 코드 복사
>     ![](https://velog.velcdn.com/images/sieunpark/post/de20be1d-6fb5-4a87-b719-d1d5915afda2/image.png)
>     <br>3. 소스 코드 붙여넣기

```
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZeBsrkPq5dM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
```

<br>4. 완성!
![](https://velog.velcdn.com/images/sieunpark/post/fe5192c1-60fd-47b6-97fe-fcdbda5dc57d/image.png)

---

## ▷ 댓글 기능 추가

댓글 기능을 직접 구현하는 것은 백엔드라는 고도의 기술이 요구된다.
따라서 Disqus나 LiveRe이라는 사이트를 이용하여 남들이 만든 댓글을 가져올 것이다.

> 1.  GET STARTED 클릭
>     ![](https://velog.velcdn.com/images/sieunpark/post/284398e2-d24b-4955-a66c-850ce2f07430/image.png)
>     <br>2. I want to install Disqus on my site 클릭
>     ![](https://velog.velcdn.com/images/sieunpark/post/4d1181bb-fa59-425b-853e-bb9209a5edb3/image.png)
>     <br>3. Website Name 입력 후 Create Site 클릭
>     ![](https://velog.velcdn.com/images/sieunpark/post/0c2e31cb-7d94-4ee6-bb0e-d79c4a3fde3f/image.png)
>     <br>4.하단부 Universal Code 클릭
>     ![](https://velog.velcdn.com/images/sieunpark/post/89acc2ca-29fa-4482-b981-3146abbcb78a/image.png)
>     <br>5. 소스 코드 복사
>     ![](https://velog.velcdn.com/images/sieunpark/post/3d6fd4e3-f067-436f-b1b4-7487dd662ce4/image.png)
>     <br>6. 소스 코드 붙여넣기

```
<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    /*
    var disqus_config = function () {
    this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://html1-19.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

<br>7. 웹서버를 이용하여 웹페이지 열기

- 웹서버를 이용하지 않으면 보안상의 이유로 디스커스를 로드할 수 없다.
  ![](https://velog.velcdn.com/images/sieunpark/post/431e0ea7-4a35-4e42-a564-1ec491f4bca1/image.png)
- 따라서 웹 호스팅(github의 pages 기능)을 사용하여 댓글 기능을 로드하자
- 깃허브에 코드 업데이트 해줘야한다.
- 짜잔!
  ![](https://velog.velcdn.com/images/sieunpark/post/757bee75-1378-44cc-b54d-1898d8637a4b/image.png)

---

## ▷ 채팅 기능 추가

tawk 사이트를 이용하여 채팅 기능을 추가하기

> 1.  소스 코드 복사
>     ![](https://velog.velcdn.com/images/sieunpark/post/b1b7b3b9-3164-4cbb-b2f4-2f311f8b826d/image.png)
>     <br>2. 소스 코드 붙여넣기

```
  <!--Start of Tawk.to Script-->
<script type="text/javascript">
  var Tawk_API=Tawk_API||{}, Tawk_LoadStart=new Date();
  (function(){
  var s1=document.createElement("script"),s0=document.getElementsByTagName("script")[0];
  s1.async=true;
  s1.src='https://embed.tawk.to/63d25715c2f1ac1e202fb28f/1gnmq8bnb';
  s1.charset='UTF-8';
  s1.setAttribute('crossorigin','*');
  s0.parentNode.insertBefore(s1,s0);
  })();
  </script>
  <!--End of Tawk.to Script-->
```

<br>3. 웹서버를 이용하여 웹페이지 열기 (위 내용 참고)
<br>4. 짜잔!
![](https://velog.velcdn.com/images/sieunpark/post/f69005b7-3541-4a62-b416-d1e8fd5bcc9e/image.jpg)

---

## ▷ 방문자 분석기

Google 애널리틱스 사이트를 이용하여 방문자 분석기능 추가하기
Google 애널리틱스 구조는 다음과 같다.
![](https://velog.velcdn.com/images/sieunpark/post/0735af20-6e97-44fc-89c4-9650ac47446c/image.png)

> 1. 웹 클릭하기
>    ![](https://velog.velcdn.com/images/sieunpark/post/ce215192-e410-4313-acef-e27bb592ed60/image.png)
>    <br>2. 웹사이트 링크와 스트림 이름 입력한 후 스트림 만들기 클릭
>    ![](https://velog.velcdn.com/images/sieunpark/post/65dc1c61-7697-4d8e-af04-4722101cd234/image.png)
>    <br>3. 코드 복사하기
>    ![](https://velog.velcdn.com/images/sieunpark/post/f9641b96-331b-4189-bbf3-f033c72a8278/image.png)
>    <br>4. 코드 붙여넣기

```
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-Y5YH40CBYP"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-Y5YH40CBYP');
</script>
```

<br>5. 웹서버를 이용하여 웹페이지를 열기 (위 내용 참고)
<br>6. 짜잔!
![](https://velog.velcdn.com/images/sieunpark/post/e947349b-40db-40d1-b351-1895a5d7e56e/image.png)

- 웹페이지에 데이터가 쌓이면 아래와 같이 나타난다.
  ![](https://velog.velcdn.com/images/sieunpark/post/5d6055c4-c2a7-49dd-8273-ed690bed7998/image.png)
- 위 페이지는 google anayle demo를 검색한 후 Google Analytics 4 property: Google Merchandise Store (web data) 클릭하여 볼 수 있다.
  ![](https://velog.velcdn.com/images/sieunpark/post/ccda7160-dd2e-42be-a7f3-23573a6dc457/image.png)

---

# ▶ 전체 코드

```
<!doctype html>
<html>
<head>
  <!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-Y5YH40CBYP"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-Y5YH40CBYP');
</script>
  <title>WEB1 - html</title>
  <meta charset="utf-8">
</head>
<body>
<h1><a href="index.html">WEB</a></h1>
<ol>
    <li><a href="1.html">HTML</a></li>
    <li><a href="2.html">CSS</a></li>
    <li><a href="3.html">JaveScript</a></li>
  </ol>
<h2>HTML</h2>
웹 페이지를 구성하는 가장 기초적인 <u>마크업 언어</u>이다.<br>
<p>텍스트, 이미지, 비디오, 링크 등을 어디에 배치할지 구조화하는 뼈대이며, 추후 CSS를 통해 살을 붙여준다.
<strong><a href="https://www.websiterating.com/ko/web-hosting/glossary/what-is-html/" target="_blank" title="HTML이란?">Hyper Text Markup Language</a></strong>의 약자이다.</p>
<img src="https://images.unsplash.com/photo-1515879218367-8466d910aaa4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80" width="25%">
<script>
  /**
  *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
  *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
  /*
  var disqus_config = function () {
  this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
  this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
  };
  */
  (function() { // DON'T EDIT BELOW THIS LINE
  var d = document, s = d.createElement('script');
  s.src = 'https://html1-19.disqus.com/embed.js';
  s.setAttribute('data-timestamp', +new Date());
  (d.head || d.body).appendChild(s);
  })();
</script>
<p>
  <!--Start of Tawk.to Script-->
<script type="text/javascript">
  var Tawk_API=Tawk_API||{}, Tawk_LoadStart=new Date();
  (function(){
  var s1=document.createElement("script"),s0=document.getElementsByTagName("script")[0];
  s1.async=true;
  s1.src='https://embed.tawk.to/63d25715c2f1ac1e202fb28f/1gnmq8bnb';
  s1.charset='UTF-8';
  s1.setAttribute('crossorigin','*');
  s0.parentNode.insertBefore(s1,s0);
  })();
  </script>
  <!--End of Tawk.to Script-->
</p>
</body>
</html>
```

---

# ▶ 공부후기

- C언어를 배울 때와 달리 내가 공부한 내용을 바로 적용할 수 있어서 매우매우 재밌었다! 뭔가 더 체계화된 웹 사이트를 만들 수 있을 것 같은 자신감이 생겼다.

- 특히 제일 재밌었던 부분은 댓글, 채팅 방문자 분석기를 사이트들을 이용하여 추가하는 것이였다. 당연히 이러한 기능을 구현하는 것은 굉장히 심오하고 많은 공부가 필요할 줄 알았는데 백엔드 기술을 배우지 않아도 구현할 수 있다는 점이 신기했고 백엔드 기술을 배워보고 싶다는 생각이 들었다.

- 검색 능력도 코딩 실력의 한 부분이라고 한 말을 들은 적 있다. 이번에 디스커스를 비롯한 편의를 위해 나온 사이트들을 이용하면서 내가 처음부터 코드를 쌓아가는 것 보다 이미 만들어진 코드들을 검색을 통해 적절히 결합하여 하나의 웹사이트를 만드는 것이 중요하다는 것을 깨달았다.

- 이 강의에서 어려웠던 부분은 딱히 없었다. 하지만 막상 완성된 웹페이지를 보니 만들기 전 기획한 웹페이지와 많이 달랐다. 글꼴도 바꾸고싶고 기획안처럼 레이아웃을 나누고 싶은데 HTML로는 한계가 있는 것이다.

- 그래서 기획안과 최대한 똑같이 하기 위해 필요한 기술인 CSS를 공부해야겠다고 생각했다.

![](https://velog.velcdn.com/images/sieunpark/post/c339fe97-b613-4947-806b-2556aa1b5345/image.jpg)
