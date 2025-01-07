---
title: "[TS] #4 React에서 TS 시작하기 (Vite 사용)"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. npx 사용

## 1.1 새로운 폴더에 프로젝트 생성

<span style="color:indianred">⚠️</span> 프로젝트 이름은 소문자만 사용이 가능하다!

```shell
npx create-react-app 디렉토리명 --template typescript
```

<br>

## 1.2 현재 폴더에 프로젝트 생성

> `./`을 입력해주면 된다.

```shell
npx create-react-app ./ --template typescript
```

![](/assets/images/2024/2024-09-18-16-31-00.png)

<br><br>

# 2. Vite 사용(추천)

[[Vite 문서↗️]](https://ko.vitejs.dev/)
{: .notice--danger}

## 2.1 Vite의 특징

- 간편한 설정
  - 기본 설정이 최소화되어 있어, 별도의 복잡한 설정 없이 바로 프로젝트를 시작할 수 있다.
- 빠른 개발 서버 시작
  - Vite는 ES 모듈을 활용하여 필요한 파일만 즉시 가져오기 때문에, 개발 서버가 매우 빠르게 시작된다.
- 프레임워크 통합
  - Vite는 Vue, React, Preact, Svelte 등 다양한 프레임워크와의 통합을 지원한다.

<br>

## 2.2 프로젝트 생성

> 터미널에서 아래 명령어를 입력하여 Vite 프로젝트를 생성하자.

```
npm create vite
or
yarn create vite
```

- 위 명령어를 실행하면 프로젝트 이름과 템플릿을 선택하는 옵션이 나타나는데, 만약 프로젝트를 현재 디렉토리에 생성하고 싶다면, 프로젝트 이름으로 `.`을 입력하면 된다.
- `TypeScript`와 `TypeScript + SWC` 의 차이는 컴파일러의 컴파일 속도이다. SWC를 사용하여 TypeScript를 컴파일하면, tsc를 사용하는 것보다 더 빠르게 컴파일할 수 있다.<br><br>
  ![](/assets/images/2024/2024-09-18-16-35-50.png)
  ![](/assets/images/2024/2024-09-18-16-36-11.png)

<br>

## 2.3 프로젝트 설정

> 프로젝트 생성이 완료되면, 생성한 디렉토리로 이동하여 종속성을 설치하자.

```shell
cd 프로젝트_이름
npm install
or
yarn install
```

<br><br>

## 2.4 스크립트 설정

> `package.json` 파일을 열어 스크립트를 확인하고, 필요에 따라 수정하면 된다.

```jsx
{
  "scripts": {
    "dev": "vite", // 개발 서버를 실행 (`vite dev` 또는 `vite serve`로도 시작이 가능)
    "build": "vite build", // 배포용 빌드 작업을 수행
    "preview": "vite preview" // 로컬에서 배포용 빌드에 대한 프리뷰(배포 결과 미리보기) 서버를 실행
  }
}
```

<br>

## 2.5 프로젝트 실행

> 터미널에서 아래 명령어를 입력하여 개발 서버를 시작하면 된다.

```shell
npm run dev
or
yarn dev
```

![](/assets/images/2024/2024-08-04-20-10-26.png)

<br>
