---
title: "[Git] Github Convention"
categories: [Git]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 깃헙 커밋 규칙

> `[타입] - 하려는 내용` 형태로 작성하기,

## 커밋 타입 설명

| **타입** | **설명**                                          | **예시**                                              |
| -------- | ------------------------------------------------- | ----------------------------------------------------- |
| feat     | 기능 구현                                         | [feat] - 페인페이지 레이아웃 구현                     |
| rename   | 파일/폴더 이름 변경 및 이동                       | [rename] - `src/old-folder`를 `src/new-folder`로 이동 |
| script   | 라이브러리 추가                                   | [script] - `supabase` 라이브러리 추가                 |
| fix      | 버그 수정                                         | [fix] - `supabase` env 미연결 문제 해결               |
| chore    | 빌드 업무 수정, 패키지 매니저 설정 수정           | [chore] - .env 설정 변경                              |
| refactor | 코드 리팩토링                                     | [refactor] - 함수 분리 및 코드 정리                   |
| style    | 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우 | [style] - 버튼 스타일 수정                            |
| test     | 테스트 코드, 리팩토링 테스트 코드 추가            | [test] - 유저 로그인 기능 테스트 추가                 |
| docs     | 문서 수정                                         | [docs] - API 문서 업데이트                            |

<br><br>

# 2. 브랜치 네이밍

| 브랜치 유형 | 네이밍 규칙                                                     |
| ----------- | --------------------------------------------------------------- |
| **main**    | `main`                                                          |
| **develop** | `dev`                                                           |
| **feature** | `feat/{기능 요약}`<br>또는<br>`feat/{issue-number}-{기능 요약}` |
| **release** | `release-{버전}`                                                |
| **hotfix**  | `hotfix-{버전}`                                                 |

<br><br>

# 3. 템플릿 정하기

## 3.1 issue 템플릿 정하기

[[GitHub Issue 사용하여 협업하기↗️]](https://mynamesieun.github.io/git/GitHub-Issue-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%ED%98%91%EC%97%85%ED%95%98%EA%B8%B0/)

<br>

## 3.2 PR 템플릿 정하기

[[GitHub PR template 만들고 사용하기↗️]](https://mynamesieun.github.io/git/GitHub-PR-template-%EB%A7%8C%EB%93%A4%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/)

<br><br>

## 4. PR 규칙

> PR (Pull Request) 하기 전

- 커밋은 가능하면 자주 할 것
- 본인 github 브랜치에 push도 가능하면 자주 할 것
- PR은 너무 뜸하지 않게 주기적으로 실시할 것 (주기가 너무 뜸하면, 그만큼 검토해야할 코드가 늘어나기 때문에 그만큼 시간이 지연됨)

<br>

> PR (Pull Request) 주의사항

- **(중요) 반드시 검토할 사람이 있을 때 PR을 올릴 것!!**
- 라이브러리를 추가한 경우, 어떤 라이브러리가 추가되었는지 PR 본문에 작성할 것
- 공통적인 부분 (ex. Route, 공통 컴포넌트, 공통 스타일 등) 을 수정한 경우, 어떤 부분을 수정했는지 PR 본문에 작성할 것

<br>

> PR (Pull Request) 수행 이후에 할 일

1.  Slack에 PR URL 주소를 올림
2.  다른 사람들이 PR을 검토하고 "승인" 하면 Merge 진행
3.  Slack에 Merge 했다고 메시지를 남김

<br>

> Merge 수행 이후에 할 일

1.  Merge 성공 여부를 검토할 사람이 본인 브랜치에서 작업하던 중간 지점까지 commit
2.  `git pull origin dev`
3.  추가된 라이브러리가 있는 경우 `yarn`
4.  `yarn start` 하여 충돌 여부 확인
5.  충돌 없이 정상 작동 시 `"확인했다"` , `"이상 없다"` 같은 내용을 Slack에 남기기<br>
    충돌이 있을 시 `"어떤 부분에서 충돌이 있다"` , `"충돌이 있었는데 이렇게 해결했다"` 같은 내용을 Slack에 남기기

<br>
