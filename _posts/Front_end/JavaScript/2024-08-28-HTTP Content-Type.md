---
title: "[JS] HTTP Content-Type"
categories: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. HTTP Content-Type

## 1.1 HTTP Content-Type 헤더

> HTTP Content-Type 헤더는 클라이언트(보통 웹 브라우저)와 서버 간의 통신에서 전송되는 데이터의 타입을 명시하는 데 사용된다.

- 이 헤더는 서버가 클라이언트에게 응답을 보낼 때 응답 본문의 콘텐츠가 어떤 형식인지 알려준다.
- 예를 들어, 웹 브라우저가 서버로부터 데이터를 받을 때 이 헤더를 참조하여 해당 데이터를 어떻게 처리할지 결정한다.

<br>

## 1.2 Content-Type의 역할

> 클라이언트에서 서버로 요청 시

- 클라이언트는 Content-Type 헤더를 설정하여 서버에 어떤 형식의 데이터를 전송하고 있는지 알린다.
- 예를 들어, JSON 데이터를 전송할 때는 Content-Type: application/json을 사용한다.

> 서버에서 클라이언트로 응답 시

- 서버는 응답 본문을 보낼 때 Content-Type을 설정하여 클라이언트가 데이터를 올바르게 해석할 수 있도록 한다.
- 예를 들어, HTML 페이지를 응답할 때는 Content-Type: text/html을 사용하여 브라우저가 HTML로 렌더링할 수 있도록 한다.

<br>

> Content-Type을 설정하지 않았을 경우, `application/x-www-form-urlencoded; charset=UTF-8` 타입으로 지정된다.

따라서 Content-Type 헤더를 명시하여 데이터의 해석을 명확히 해야한다.

<br>

## 1.3 Content-Type의 형식

> Content-Type 헤더는 다음과 같은 형식으로 지정된다.

```
Content-Type: type/subtype; charset=UTF-8
```

- `type/subtype`
  - MIME 타입으로, 데이터의 종류를 나타낸다.
  - 예를 들어, text/html, application/json, image/png 등이 있다.
- `charset`
  - 선택 사항으로, 텍스트 데이터의 문자 인코딩을 나타낸다.
  - 예를 들어 charset=UTF-8은 UTF-8 인코딩을 의미한다.

<br>

# 2. Content-Type 에 들어가는 MIME type

## 2.1 MIME 타입

> HTTP Content-Type 헤더의 값에는 MIME type(또는 media type) 이 들어간다.

- MIME 타입은 파일의 형식과 콘텐츠 유형을 정의하는데, 타입과 서브타입으로 나뉜다.
- 타입은 데이터의 주요 카테고리를, 서브타입은 그 카테고리 내의 구체적인 형식을 나타낸다.
  - 타입: 데이터의 주요 카테고리 (예: text, image, audio)
  - 서브타입: 카테고리 내의 구체적인 형식 (예: plain, jpeg, mp3)
  - 예시: text/html, image/png, application/json
- HTML 폼을 통해 데이터를 제출할 때 Content-Type을 명시하지 않으면, 기본적으로 application/x-www-form-urlencoded로 처리된다.

## 2.2 주요 MIME 타입

<details>
  <summary>더 많은 MINE 타입 확인하기</summary>

```
1. Multipart Related MIME 타입

- Content-Type: Multipart/related <-- 기본형태
- Content-Type: Application/X-FixedRecord

2. XML Media의 타입

- Content-Type: text/xml
- Content-Type: Application/xml
- Content-Type: Application/xml-external-parsed-entity
- Content-Type: Application/xml-dtd
- Content-Type: Application/mathtml+xml
- Content-Type: Application/xslt+xml

3. Application의 타입

- Content-Type: Application/EDI-X12 <-- Defined in RFC 1767
- Content-Type: Application/EDIFACT <-- Defined in RFC 1767
- Content-Type: Application/javascript <-- Defined in RFC 4329
- Content-Type: Application/octet-stream : <-- 디폴트 미디어 타입은 운영체제 종종 실행파일, 다운로드를 의미
- Content-Type: Application/ogg <-- Defined in RFC 3534
- Content-Type: Application/x-shockwave-flash <-- Adobe Flash files
- Content-Type: Application/json <-- JavaScript Object Notation JSON; Defined in RFC 4627
- Content-Type: Application/x-www-form-urlencode <-- HTML Form 형태

* x-www-form-urlencode와 multipart/form-data은 둘다 폼 형태이지만
  x-www-form-urlencode은 대용량 바이너리 테이터를 전송하기에 비능률적이기 때문에
  대부분 첨부파일은 multipart/form-data를 사용하게 된다.

4. 오디오 타입

- Content-Type: audio/mpeg <-- MP3 or other MPEG audio
- Content-Type: audio/x-ms-wma <-- Windows Media Audio;
- Content-Type: audio/vnd.rn-realaudio <-- RealAudio; 등등

5. Multipart 타입

- Content-Type: multipart/mixed: MIME E-mail;
- Content-Type: multipart/alternative: MIME E-mail;
- Content-Type: multipart/related: MIME E-mail <-- Defined in RFC 2387 and used by MHTML(HTML mail)
- Content-Type: multipart/form-data <-- 파일 첨부

6. TEXT 타입

- Content-Type: text/css
- Content-Type: text/html
- Content-Type: text/javascript
- Content-Type: text/plain
- Content-Type: text/xml

7. file 타입

- Content-Type: application/msword <-- doc
- Content-Type: application/pdf <-- pdf
- Content-Type: application/vnd.ms-excel <-- xls
- Content-Type: application/x-javascript <-- js
- Content-Type: application/zip <-- zip
- Content-Type: image/jpeg <-- jpeg, jpg, jpe
- Content-Type: text/css <-- css
- Content-Type: text/html <-- html, htm
- Content-Type: text/plain <-- txt
- Content-Type: text/xml <-- xml
- Content-Type: text/xsl <-- xsl
```

</details>

> 텍스트 (text)

| MIME 타입  | 설명            |
| ---------- | --------------- |
| text/plain | 일반 텍스트     |
| text/html  | HTML 문서       |
| text/css   | CSS 스타일 시트 |

> 이미지 (image)

| MIME 타입  | 설명        |
| ---------- | ----------- |
| image/jpeg | JPEG 이미지 |
| image/png  | PNG 이미지  |
| image/gif  | GIF 이미지  |

> 비디오 (video)

| MIME 타입        | 설명       |
| ---------------- | ---------- |
| video/mp4        | MP4 비디오 |
| video/x-matroska | MKV 비디오 |

> 오디오 (audio)

| MIME 타입  | 설명       |
| ---------- | ---------- |
| audio/mpeg | MP3 오디오 |
| audio/wav  | WAV 오디오 |

> 애플리케이션 (application)

| MIME 타입                | 설명                 |
| ------------------------ | -------------------- |
| application/json         | JSON 데이터          |
| application/xml          | XML 데이터           |
| application/octet-stream | 일반 바이너리 데이터 |

> 멀티파트 (multipart)

| MIME 타입             | 설명                                        |
| --------------------- | ------------------------------------------- |
| multipart/form-data   | 파일 업로드와 같은 복합 폼 데이터           |
| multipart/alternative | 여러 형식의 콘텐츠를 포함하는 이메일 메시지 |

<br>

# 3. 예시

## 3.1 JSON 데이터 전송

서버에 JSON 형식의 데이터를 전송할 때 Content-Type을 application/json으로 설정한다.

```jsx
import axios from "axios";

const fetchData = async () => {
  try {
    const response = await axios.post(
      "/api/data",
      { key: "value" },
      {
        headers: { "Content-Type": "application/json" },
      }
    );
    console.log(response.data); // 응답 데이터 처리
  } catch (error) {
    console.error(
      "Error:",
      error.response ? error.response.data : error.message
    ); // 오류 처리
  }
};

fetchData();
```

<br>

## 3.2 폼 데이터 전송

- 폼 데이터를 전송할 때는 Content-Type을 application/x-www-form-urlencoded로 설정한다.
- qs 라이브러리와 같은 URL 인코딩 도구를 사용할 수 있다.

```jsx
import axios from "axios";
import qs from "qs";

const submitForm = async () => {
  try {
    const response = await axios.post(
      "/api/submit",
      qs.stringify({ key1: "value1", key2: "value2" }),
      { headers: { "Content-Type": "application/x-www-form-urlencoded" } }
    );
    console.log(response.data);
  } catch (error) {
    console.error(
      "Error:",
      error.response ? error.response.data : error.message
    );
  }
};

submitForm();
```

<br>

## 3.3 파일 업로드

파일 업로드를 위한 요청에서는 Content-Type을 multipart/form-data로 설정한다.

```jsx
import axios from "axios";

const uploadFile = async (file) => {
  const formData = new FormData();
  formData.append("file", file);

  try {
    const response = await axios.post("/api/upload", formData, {
      headers: { "Content-Type": "multipart/form-data" },
    });
    console.log(response.data);
  } catch (error) {
    console.error(
      "Error:",
      error.response ? error.response.data : error.message
    );
  }
};

// 예시: fileInput은 파일 입력 요소의 참조
const fileInput = document.querySelector('input[type="file"]');
fileInput.addEventListener("change", () => {
  if (fileInput.files.length > 0) {
    uploadFile(fileInput.files[0]);
  }
});
```

<br>

# 참조

- [http content-type 관한 정리](https://yunzema.tistory.com/186)
- [https://jake-seo-dev.tistory.com/478 [제이크서 위키 블로그:티스토리]](https://jake-seo-dev.tistory.com/478)
  RESTful API와 같은 서버와의 상호작용에서는 Content-Type을 명시하는 것이 필수적입니다. API는 요청 본문의 데이터 형식을 정확히 알고 있어야 하며, 적절히 처리할 수 있습니다.

<br>
