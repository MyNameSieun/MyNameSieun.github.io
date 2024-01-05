---
title: "[etc] Google Material Icons 적용하기"
categories: [etc]
tag: [etc, Icons]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

1. HTML `<head>` 태그 내에 아래 `<link>` 태그를 작성한다.
   [구글 머티리얼 아이콘 가이드](https://developers.google.com/fonts/docs/material_icons?hl=ko)

```jsx
<link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">
```

<br>

2. 구글아이콘 검색하고 소스코드 복사하기
   [Google Material Icons](https://fonts.google.com/icons) 링크
   ![](https://velog.velcdn.com/images/sieunpark/post/4cc9ab29-d3fc-4a3a-a95c-c76e3c2725e4/image.png)

<br>

머티리얼 아이콘 사용 중에 아이콘이 적용되지 않는 문제가 있었는데, 복사한 태그 중 outlined를 제거하니 해결됐다!

<br>

---

<br>

# 📎참조

- https://www.inflearn.com/blogs/1039
