---
title: "[Project] Brave lion: 멋쟁이사자처럼 지원 웹페이지 만들기"
categories: [Project]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

![![Brave lion](image.png)
](<../../../assets/images/2024/Brave lion.gif>)

> [[웹페이지 Brave lion]](https://mynamesieun.github.io/Brave-lion/)

# ▶ 프로젝트 기획 동기🦁

> - 멋쟁이사자처럼 11기 아기사자가 되고자하는 열정을 보여주기 위해 웹페이지를 제작하였습니다.
>   공부한 기술이 있을 때마다 업데이트해 나가고 있습니다.

> 멋쟁이사자처럼 지원 웹페이지 만들기라고 타이틀을 설정해놓았지만, 사실 공부한 내용을 적용하기 위한 웹사이트에 가깝습니다. 그래서 혹여나 멋사 11기에 합격하지 못하더라도 공부한 내용을 적용하며 계속해서 업데이트해 나갈 예정입니다!

---

# ▶ UI 디자인🎨

## ▷ 페이지 스케치

> 삼성노트와 포토샵을 통해 코드를 작성하기 전 페이지 디자인을 해주었습니다.
> 미리 스케치를 하고 코드를 작성하니 빠르게 코드를 작성할 수 있었습니다.

![](https://velog.velcdn.com/images/sieunpark/post/2e454315-15f8-45b7-ac09-7f6abc0c45bb/image.png)![](https://velog.velcdn.com/images/sieunpark/post/d71204a6-3591-40ce-b5fc-33a1f5272eb2/image.png)

<br>

## ▷ 페이지 컬러

> 멋쟁이사자처럼을 대표하는 동물이 사자인만큼 색상 조합은 사자 느낌이 날 수 있도록 베이지색으로 구성하였습니다.

![](https://velog.velcdn.com/images/sieunpark/post/0332abb4-d590-4faa-92cf-41f9db57d34d/image.png)

---

# ▶ 구현한 기능🛠️

> 코드를 작성하기 전 아래의 코드로 CSS의 기본값들을 없애고 코드를 작성하였습니다.

```
* {
  list-style: none; /* 목록 점 없애기 */
  text-decoration: none; /* 텍스트색깔,밑줄 없애기 */
  border-collapse: collapse; /* 테이블 선 붙이기 */
  color: #000; /* 글자 색깔을 검정색으로 변경 */
  margin: 0px;
  padding: 0px;
}
```

<br>

## ▼ header

![](https://velog.velcdn.com/images/sieunpark/post/bb962eeb-f75a-4fd7-9cbf-85b81ac33b9b/image.png)

### ▷ flex를 이용하여 header 배치하기

> - justify-content: space-around; 를 사용하여 header의 레이아웃을 구성하였습니다.

- align-items: center; 을 사용해 수직으로 중앙정렬을 해주었습니다.

```
.navbar {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
```

<br>

### ▷ 마우스 커서 올렸을 때 폰트 색상 변경하기

![](https://velog.velcdn.com/images/sieunpark/post/c1b19c85-0bb5-4f6e-857b-d86dc0c854ff/image.gif)

> a:hover을 이용해 링크 텍스트에 마우스 커서를 올렸을 때 폰트 색상과 두께를 변경하도록 하였습니다.

```
.navbar_menu > ol > li > a:hover {
  color: #b85c38;
  font-weight: bold;
}
```

<br>

### ▷ 링크 클릭시 새 창으로 열기

> a href 속성에 <target="\_blank"> 추가하여 클릭시 새 창에서 열리도록 하였습니다.

```
<a href="https://연결할 링크" target='_blank'>
```

<br>

### ▷ 메뉴바 상단 고정하기

![](https://velog.velcdn.com/images/sieunpark/post/53b51ca2-6be8-454c-ad37-6525865a842e/image.gif)

> position: fixed; 을 사용하여 메뉴바를 상단에 고정하였습니다.

```
.navbar {
  position: fixed;
}
```

<br>

### ▷ 같은 페이지에서 해당 컨텐츠로 이동하기

![](https://velog.velcdn.com/images/sieunpark/post/008cde96-e58c-4c59-8dec-fea7bdace466/image.gif)

> `<a href="#id이름"> ex)만들고 싶은 서비스 </a>`을 작성한 후 이동하고싶은 section에 id값을 부여하여 링크를 클릭했을 때 페이지 이동이 되게 했습니다.

```
<div class="navbar_menu">
	<ol>
		<li><a href="#service">만들고 싶은 서비스</a></li>
	</ol>
</div>
```

```
<section id="service">
	<div class="service">
   	 <p><img src="images.png">HAPPY!</p>
	</div>
</section>

```

---

## ▼ 메인 페이지

![](https://velog.velcdn.com/images/sieunpark/post/574812a1-ef6c-4d1c-9807-293ab3b87d9d/image.png)

### ▷ 텍스트와 사진 가운데 배치하기

> flex의 justify-content, align-items 속성을 이용하여 텍스트와 사진을 정가운데에 배치하였습니다.

```
.main_page {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

<br>

### ▷ 글씨 디자인 다르게 설정하기

> - `<span>` 태그로 다르게 설정하고 싶은 텍스트를 감싸준 후 nth 선택자를 사용하여 글씨 크기와 굵기를 다르게 하였습니다.

- nth 선택자 사용 방법 보기 > [nth 선택자](https://velog.io/@sieunpark/nth-선택자)

- HTML

```
<div class="main_page_text">
	<p>안녕하세요!</p>
	<p><span>프론트엔드 개발자</span>를 꿈꾸는</p>
	<p>멋쟁이사자처럼 11기 지원자</p>
	<p>아기사자 꿈나무</p>
	<p><span>박시은</span> 입니다!</p>
</div>

```

- CSS

```
.main_page_text {
  margin-left: 58px;
}
.main_page_text p:nth-child(-n + 3) {
  font-size: 36px;
}
.main_page_text p:nth-child(4) {
  font-size: 48px;
  font-weight: bold;
}
.main_page_text p:nth-child(5) {
  font-size: 36px;
}
.main_page_text > p > span {
  font-weight: bold;
  font-size: 40px;
}
```

---

## ▼ 자기소개 페이지

### ▷ 스트라이프 형태로 레이아웃 구성하기

![](https://velog.velcdn.com/images/sieunpark/post/77117bce-1a62-4fea-91b8-b5e9b6097f37/image.png)

> nth선택자를 사용하여 전체 웹 사이즈 100vh를 25vh씩 쪼개어 배경 색을 다르게 바꿔주었습니다.

- HTML

```
    <section id="introduce">
        <div class="introduce_text">
            <p><img src=images.png>경영학과에 재학 중이지만 코딩에 대한 관심과<br>열정은 공대생에 뒤쳐지지 않습니다!</p>
            <p>현재 HTML와 CSS, Javascript를<br>독학으로 공부하고 있습니다<img src=images.png></p>
            <p><img src=images.png>저는 긍정적인 마인드와 풍부한 아이디어를<br>지닌 창의적인 성격의 소유자입니다</p>
            <p>노션, 다이어리, 블로그, 아이디어 노트 총 4가지의 기록을<br>작성하며 능동적인 삶을 살아가기 위해 노력하고 있습니다<img src=images.png></p>
        </div>
    </section>

```

- CSS

```
.introduce_text > p:nth-child(2n + 1) {
  background-color: #dbd0c0;
  height: 25vh;
  width: 100%;
}
.introduce_text > p:nth-child(2n) {
  background-color: #faeee0;
  height: 25vh;
  width: 100%;
}
```

---

## ▼ 지원동기, 포부 페이지

![](https://velog.velcdn.com/images/sieunpark/post/1d224d79-d5cf-410f-9e22-af6d1a4213ad/image.png)

### ▷ 유튜브 동영상 삽입하기

> - `<iframe>` 태그를 사용하면 웹페이지 안에 또 다른 HTML페이지를 삽입할 수 있습니다.

- 유튜브 동영상 삽입 방법은 아래와 같습니다.

1. 삽입하고 싶은 동영상 공유하기 버튼 누른 후 퍼가기 클릭
   ![](https://velog.velcdn.com/images/sieunpark/post/488ff96e-1212-4bc0-9dc3-f6bb7adf8511/image.png)

<br>2. 소스 코드 복사 후 원하는 페이지에 소스 코드 붙여넣기
![](https://velog.velcdn.com/images/sieunpark/post/2574d567-51dc-4b98-b86f-748b495feab5/image.png)

---

## ▼ 만들고싶은 서비스 페이지

### ▷ Grid를 사용하여 레이아웃 구성하기

![](https://velog.velcdn.com/images/sieunpark/post/a78f3e1b-766b-4c93-b644-5b531cfb5a5b/image.png)

> - Grid를 사용하여 위와 같이 레이아웃을 디자인하였습니다.

- Flex 사용 방법 보기 > [Flex](https://velog.io/@sieunpark/Flex-box-레이아웃-스타일링)
- Grid 사용 방법 보기 > [Grid](https://velog.io/@sieunpark/Grid-레이아웃-스타일링)

- CSS

```
.service {
  background-color: #faeee0;
  width: 100%;
  height: 100vh;
  display: grid;
  grid-template-columns: repeat(3, 300px);
  grid-template-rows: repeat(2, 280px);
  grid-gap: 30px;
  justify-content: center;
  align-content: center;
}


.service > p {
  background-color: #dbd0c0;
  width: 300px;
  height: 278px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  border-radius: 30px; /* 테두리를 둥글게 만드는 속성 */

}
```

---

## ▶ 댓글 기능 추가하기

![](https://velog.velcdn.com/images/sieunpark/post/51734d48-24b0-4862-9059-55c044bfa0fa/image.gif)

> - footer위에 Disqus를 이용하여 댓글 기능을 추가하였습니다.

- 댓글 기능을 추가하는 방법은 아래와 같습니다.

1. GET STARTED 클릭
   ![](https://velog.velcdn.com/images/sieunpark/post/284398e2-d24b-4955-a66c-850ce2f07430/image.png)
   <br>2. I want to install Disqus on my site 클릭
   ![](https://velog.velcdn.com/images/sieunpark/post/4d1181bb-fa59-425b-853e-bb9209a5edb3/image.png)
   <br>3. Website Name 입력 후 Create Site 클릭
   ![](https://velog.velcdn.com/images/sieunpark/post/0c2e31cb-7d94-4ee6-bb0e-d79c4a3fde3f/image.png)
   <br>4.하단부 Universal Code 클릭
   ![](https://velog.velcdn.com/images/sieunpark/post/89acc2ca-29fa-4482-b981-3146abbcb78a/image.png)
   <br>5. 소스 코드 복사
   ![](https://velog.velcdn.com/images/sieunpark/post/3d6fd4e3-f067-436f-b1b4-7487dd662ce4/image.png)
   <br>6. 소스 코드 붙여넣기

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

---

# ▶ 구현해야할 기능⭐🥕

> 현재 자바스크립트를 공부중에 있습니다.
> 공부 후 아래 기능들을 추가로 구현하며 웹페이지를 업데이트 할 예정입니다.

1. 상단이동버튼 넣기
2. 가로 진행바 넣기
3. 글자 타이핑 효과 부여하기
4. 미디어 쿼리를 사용하여 반응형 웹페이지로 만들기

---

# ▶ 느낀점🐥

> Html과 CSS만 공부하고 웹페이지를 만들어보니 사용자와 상호작용 할 수 있는 다양한 기능들을 추가하고 싶다는 생각을 하게 되었습니다. 얼른 JavaScript를 공부하여 이 웹사이트에 다채로운 기능을 추가하여 완성도있는 웹사이트를 만들고 싶습니다🥰
