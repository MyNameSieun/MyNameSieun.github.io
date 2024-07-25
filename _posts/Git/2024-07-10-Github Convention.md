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

> `[타입] - 하려는 내용` 형태로 작성하기

| 타입     | 설명                         |
| -------- | ---------------------------- |
| feat     | 기능 구현                    |
| rename   | 파일/폴더 이름 변경 및 이동  |
| script   | 라이브러리 추가              |
| fix      | 버그 수정                    |
| chore    | 주석 추가/삭제, console 제거 |
| refactor | 코드 리팩토링                |
| style    | CSS 코드                     |
| test     | 테스트 코드                  |
| docs     | 문서 수정                    |

> 예시

|      예시       |                 설명                  |
| :-------------: | :-----------------------------------: |
|    기능 구현    |   [feat] - 페인페이지 레이아웃 구현   |
| 라이브러리 추가 |  [script] - supabase 라이브러리 추가  |
|    버그 수정    | [fix] - supabase env 미연결 문제 해결 |

<br>

# 2. 이슈(Issue) 작성 규칙

## 2.1 이슈 생성하기

> ① 이슈 생성하기

GitHub 리포지토리에서 상단의 "Issues" 탭을 클릭하고 New issue" 버튼을 클릭하여 새 이슈를 작성

![](/assets/images/2024/2024-07-25-21-23-22.png)

<br>

## 2.2 이슈 작성하기

> 이슈 제목(Title)

- "나 이런 거 할 거다." 를 이슈의 제목으로 입력
- 위의 깃헙 커밋 규칙 의 타입을 참고하여 `[타입] - 하려는 내용` 형태의 이슈 제목 작성
- `Assignees` 를 클릭하여 담당자(자기 자신) 지정

<br>

> 이슈 본문

- Issue Feature : 하려는 내용
- Todo : 수행할(→ 수행한) 주요 작업 리스트 (커밋 1개당 Todo 1개를 작성하는 게 아님)

  ```markdown
  제목 : [feat] - 메인페이지 레이아웃 구현

  Issue Feature : 메인페이지 레이아웃 구현

  Todo

  - [] MainPage.jsx 생성
  - [] Router에 MainPage 연결 // Router처럼 모두가 사용하는 기능을 조작하는 경우 반드시 Todo에 작성
  - [] 세부 컴포넌트 생성
  ```

<br>

> 이슈에 커밋(commit)을 반영하는 방법

- 이슈를 생성하면 해당 이슈의 번호(ex. `#12` )가 생기므로 해당 번호를 활용
- 커밋 메시지를 `[커밋 타입/#이슈번호] - 커밋 내용` 형태로 작성
  (ex. `[feat/#12] - Router에 MainPage 연결` )

> 이슈의 Todo를 모두 끝낸 경우

- PR (Pull Request) 할 때 PR 본문에 `키워드 #이슈번호` 입력
- 키워드 종류
  - `close` / `closes` / `close`
    `fix` / `fixes` / `fixed`
    `resolve` / `resolves` / `resolved`

<br>

# 3. PR (Pull Request), Pull 규칙

> PR template

[Github - Pull request template 작성과 설정↗](https://green-bin.tistory.com/16)

<br>

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
5.  충돌 없이 정상 작동 시 `"확인했다"` , `"이상 없다"` 같은 내용을 Slack에 남기기
    충돌이 있을 시 `"어떤 부분에서 충돌이 있다"` , `"충돌이 있었는데 이렇게 해결했다"` 같은 내용을 Slack에 남기기

<br>
