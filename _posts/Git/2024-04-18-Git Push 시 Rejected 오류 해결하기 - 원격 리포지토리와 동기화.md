---
title: "[Git] Git Push 시 Rejected 오류 해결하기: 원격 리포지토리와 동기화"
categories: [Git]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> 문제상황

로컬 저장소에 있는 프로젝트를 깃허브 사이트를 통해 만든 저장소로 push하고자 할 때 아래와 같은 오류 메시지가 발생하였다.

![](/assets/images/2024/2024-04-18-14-38-26.png)

- `non-fast-forward`
  - 로컬 브랜치와 원격 저장소의 변경 이력이 충돌하여 Git이 이를 해결할 수 없다는 것을 의미
  - 즉, 원격 리포지토리와 로컬 리포지토리 차이로 인한 문제이다.

<br>

> 해결방안

이를 해결하기 위해서는 먼저 로컬 브랜치를 최신 상태로 업데이트하고, 그 후에 push를 하면 된다.

1. git pull 시에 `–allow-unrelated-histories` 옵션을 추가하여 관련 없었던 두 저장소를 병합하도록 허용하면 된다.

```shell
git pull origin main --allow-unrelated-histories
```

2. 그 후, commit 메시지 작성 후 push 하여 변경 사항을 원격 저장소에 업데이트 하면 된다.

➡️ 이렇게 하면 "non-fast-forward" 오류를 해결하고, 로컬 브랜치와 원격 저장소의 변경 이력을 통합할 수 있다.

<br>

# 참조

- [Git push가 안되는 경우](https://gdtbgl93.tistory.com/63)
- [깃허브 non-fast-forward 에러 해결하기](https://velog.io/@rain98/%EA%B9%83%ED%97%88%EB%B8%8C-non-fast-forward-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)
