---
title: "[Git] GitHub README.md 작성 가이드"
categories: [Git]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

````markdown
<div id="top"></div>

<div align='center'>

<h1><b>프로젝트 제목</b></h1>
<h3><b>프로젝트 부제목</b></h3>

🔗 [배포 링크](https://)

<img src="" alt="intro title image"/>

</div>

<br>

## 0. 목차

1. [프로젝트 소개](#1)
2. [팀원 소개](#2)
3. [개발 일정](#3)
4. [기술 스택](#4)
5. [라이브러리 사용 이유](#5)
6. [컨벤션](#6)
7. [브랜치 및 디렉토리 구조](#7)
8. [주요 기능 소개](#8)
9. [상세 담당 업무](#9)
10. [주요 코드 ](#10)
11. [트러블 슈팅](#11)
12. [프로젝트 회고](#12)
13. [시작 가이드](#13)

<br />

## <span id="1">🚩 1. 프로젝트 소개</span>

Notion: [프로젝트 노션 링크](https://)

프로젝트에 대한 전반적인 소개를 여기에 적어주세요.

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="2">🏃 2. 팀원 소개</span>

<div align="center">

|                                                               [팀원1 이름](https://github.com/username1)                                                                |                                                               [팀원2 이름](https://github.com/username2)                                                                |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                              <img src="" width="120px;" alt="팀원1 사진"/>                                                              |                                                              <img src="" width="120px;" alt="팀원2 사진"/>                                                              |
| <img src="https://img.shields.io/badge/기능1-FF5727" /> <img src="https://img.shields.io/badge/기능2-365486" /> <img src="https://img.shields.io/badge/기능3-609966" /> | <img src="https://img.shields.io/badge/기능1-FF5727" /> <img src="https://img.shields.io/badge/기능2-365486" /> <img src="https://img.shields.io/badge/기능3-609966" /> |

</div>

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="3">📅 3. 개발 일정</span>

> 프로젝트 개발 기간: 2024.00.00 ~ 2024.00.00

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="4">📚 4. 기술 스택</span>

### Environment

![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white)![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

### Config

![Yarn](https://img.shields.io/badge/yarn-%232C8EBB.svg?style=for-the-badge&logo=yarn&logoColor=white)

### Development

![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)![React Query](https://img.shields.io/badge/-React%20Query-FF4154?style=for-the-badge&logo=react%20query&logoColor=white)![React Router](https://img.shields.io/badge/React_Router-CA4245?style=for-the-badge&logo=react-router&logoColor=white)![Redux](https://img.shields.io/badge/redux-%23593d88.svg?style=for-the-badge&logo=redux&logoColor=white)![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)![Styled Components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white)![Webpack](https://img.shields.io/badge/webpack-%238DD6F9.svg?style=for-the-badge&logo=webpack&logoColor=black)

### Project Management

![Github Issues]() ![Github Pull requests]()

### Design

![Pigma]()

### Hosting

![Vercel](https://img.shields.io/badge/vercel-%23000000.svg?style=for-the-badge&logo=vercel&logoColor=white)

### Communication

![Notion](https://img.shields.io/badge/Notion-%23000000.svg?style=for-the-badge&logo=notion&logoColor=white)![Discord](https://img.shields.io/badge/Discord-2D8CFF?style=for-the-badge&logo=Discord&logoColor=white)

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="5">❓ 5. 라이브러리 사용 이유</span>

각 라이브러리의 사용 이유를 설명해주세요.

> React

<br>

> Redux

<br>

> Axios

<br>

> Styled-Components

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="6">🤝 6. 컨벤션</span>

### prettier

```json
{
  "printWidth": 80,
  "tabWidth": 2,
  "singleQuote": true,
  "trailingComma": "all",
  "semi": false
}
```

### 커밋 컨벤션

```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 수정
style: 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
refactor: 코드 리팩토링
test: 테스트 추가, 테스트 리팩토링 (프로덕션 코드 변경 없음)
chore: 빌드 업무 수정, 패키지 매니저 설정 수정 (프로덕션 코드 변경 없음)
```

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## 7.<span id="7"> 🗂️ 브랜치 및 디렉토리 구조</span>

> 브랜치

- `main`:
- `dev`:
-

<br>

> 디렉토리 구조

```
.
├── src
│   ├── components
│   ├── pages
│   ├── redux
│   ├── utils
│   └── App.js
├── public
│   ├── index.html
│   └── favicon.ico
└── package.json

```

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="8">8. 💻 주요 기능 소개</span>

프로젝트의 주요 기능을 GIF를 첨부하여 설명해주세요.

### 1) 홈

| - 화면                                            | - 화면                                            | - 화면                                            |
| ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> |

### 2) 게시글

| 상세페이지 화면                                   | 게시물 작성 - ??                                  | 게시물 작성 - ??                                  |
| ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> |

### 3) 404 & 로딩 화면

| - 화면                                            | - 화면                                            | - 화면                                            |
| ------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------- |
| <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> | <img src="" alt="-화면" width="288" height="608"> |

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="9">9. 📄 상세 담당 업무</span>

### 1) 팀원1 이름

- **🎨 디자인**

  - 로고 디자인 및 이미지 제작

- **💻 화면 개발**

  - 로그인 화면
  - 검색 화면
  - 채팅 화면

- **🧑‍💻 구현 기능**

  - 로딩 페이지
    - 회원가입 후 로그인 모달이 올라오는 로딩페이지
  - 팔로워 목록 및 팔로워 취소&팔로우
    - 팔로워 목록을 getFollowerList로 서버에 요청하여 리스트 출력

- **♻️ 리팩토링**
  - 관련 설명

### 2) 팀원2 이름

- **🎨 디자인**

  - 전체적인 UI 디자인

- **💻 화면 개발**

  - 공통 헤더 네브바
  - 공통 푸터 네브바
  - 삭제 / 신고 모달창

- **👩‍💻 구현 기능**

  - 라우터 초기 셋팅
  - 게시물 등록
    - 토글 Open, Close에 따라 인풋창 높이 자동 조절
    - api 전송 한계로 인해 한 공간에 저장하여 보낼 수 있게, 데이터를 연산자로 구분하여 한줄로 전송
      이미지 추가 및 삭제 가능
  - 게시글 삭제 / 신고
    - userId를 통해 유저를 구별하여 타인의 경우 신고 기능, 본인일 경우 삭제 기능 구현

- **♻️ 리팩토링**
  - 관련 설명

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="10">✨ 10. 주요 코드</span>

<details>
<summary> 주요 코드에 대한 설명을 입력하세요. </summary>

<div>
설명

```jsx

```

</div>
</details>

<br>

<details>
<summary> 주요 코드에 대한 설명을 입력하세요. </summary>

<div>
설명

```jsx

```

</div>
</details>

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="11">🚦 11. 트러블 슈팅</span>

<details>
<summary> 트러블 슈팅을 입력하세요. </summary>

<div>

1. 문제 상황 <br />

2. 시도 <br />

3. 해결방안 <br />

</div>
</details>

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="12">12. 📝 프로젝트 회고</span>

프로젝트 진행 후 느낀 점과 개선할 점을 적어주세요. 블로그에 작성하셨다면 블로그 링크를 첨부해주세요.

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>

<br>

## <span id="13">13. 🛠️ 시작 가이드</span>

### Installation

```
$ git clone https://github.com/MyNameSieun/OH-YO.git
$ cd OH-YO
$ yarn
& yarn start
```

<br>

<!-- Top Button -->
<p style='background: black; width: 32px; height: 32px; border-radius: 50%; display: flex; justify-content: center; align-items: center; margin-left: auto;'><a href="#top" style='color: white; '>▲</a></p>
````

<br>

# 참조

- [짐넥(GYM-NECT)](https://github.com/FRONTENDSCHOOL7/final-07-gymnect)

<br>
