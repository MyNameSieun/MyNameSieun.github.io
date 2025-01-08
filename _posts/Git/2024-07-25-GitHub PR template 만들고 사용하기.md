---
title: "[Git] GitHub PR template 만들고 사용하기"
categories: [Git]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 0. Github PR (pull request)이란?

- PR(Pull Request)은 개발자가 자신의 브랜치에서 작업한 내용을 다른 브랜치에 병합(merge)하고자 할 때 먼저 검토할 수 있는 기능이다.
- PR을 통해 코드 변경 내용을 다른 팀원들에게 알리고, 코드 리뷰와 피드백을 받을 수 있다.
- PR이 승인되면 해당 브랜치의 내용이 메인 브랜치에 병합된다.

<br><br>

# 1. PR 템플릿 만들기

> ① pull_request_template.md 파일을 만든다.

PR 템플릿을 만들기 위해 github 디렉토리에서 `pull_request_template.md` 파일을 만든다.

![](/assets/images/2024/2024-07-25-23-14-52.png)

<br>

> ② 템플릿을 작성한다.

마크다운 형식으로 원하는 템플릿을 작성한다. (목차2. 템플릿 양식 참고)

<br>

> ③ Comit changes 를 클릭하여 커밋한다.

<br><br>

# 2. 템플릿 양식

```markdown
## #️⃣ Issue Number

<!--- ex) #이슈번호, #이슈번호 -->

## 📝 요약(Summary)

<!--- 변경 사항 및 관련 이슈에 대해 간단하게 작성해주세요. 어떻게보다 무엇을 왜 수정했는지 설명해주세요. -->

## 🛠️ PR 유형

어떤 변경 사항이 있나요?

- [ ] 새로운 기능 추가
- [ ] 버그 수정
- [ ] CSS 등 사용자 UI 디자인 변경
- [ ] 코드에 영향을 주지 않는 변경사항(오타 수정, 탭 사이즈 변경, 변수명 변경)
- [ ] 코드 리팩토링
- [ ] 주석 추가 및 수정
- [ ] 문서 수정
- [ ] 테스트 추가, 테스트 리팩토링
- [ ] 빌드 부분 혹은 패키지 매니저 수정
- [ ] 파일 혹은 폴더명 수정
- [ ] 파일 혹은 폴더 삭제

## 📸스크린샷 (선택)

## 💬 공유사항 to 리뷰어

<!--- 리뷰어가 중점적으로 봐줬으면 좋겠는 부분이 있으면 적어주세요. -->
<!--- 논의해야할 부분이 있다면 적어주세요.-->
<!--- ex) 메서드 XXX의 이름을 더 잘 짓고 싶은데 혹시 좋은 명칭이 있을까요? -->

## ✅ PR Checklist

PR이 다음 요구 사항을 충족하는지 확인하세요.

- [ ] 커밋 메시지 컨벤션에 맞게 작성했습니다.
- [ ] 변경 사항에 대한 테스트를 했습니다.(버그 수정/기능에 대한 테스트).
```

<br><br>

# 3. PR 생성하기

> ① "Compare & pull request" 버튼 클릭

![](/assets/images/2024/2024-07-25-23-54-57.png)

<br>

> ② "Create pull request" 버튼을 클릭하여 PR을 생성

![](/assets/images/2024/2024-07-25-23-55-34.png)

<br><br>

# 4. PR과 이슈 연결하기 (이슈 TODO가 끝난 후 CLOSE)

> PR (Pull Request) 할 때 PR 본문에 `키워드 #이슈번호` 입력하자

- 이슈 생성 방법은 [[여기↗️]](https://mynamesieun.github.io/git/GitHub-Issue-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%ED%98%91%EC%97%85%ED%95%98%EA%B8%B0/)를 참고하자
- 예를들어 `fix #1 - 버그를 수정하였습니다.`와 같이 작성하면 된다.
- 이렇게 하면 해당 PR이 머지될 경우 자동으로 해당 이슈가 자동으로 Close되기 때문에, 굳이 이슈에 들어가 Close 상태로 전환하지 않아도 된다.
  <br><br>
  ![](/assets/images/2024/2024-07-26-00-00-27.png)

<br>

> 커밋과 함께 이슈를 Close 할 수 있는 Keyword는 다음과 같다.

```
close / closes / close
fix / fixes / fixed
resolve / resolves / resolved
```

<br><br>

# 🔗 참조

- [Github - Pull request template 작성과 설정](https://green-bin.tistory.com/16#pull_request_template.md%20%ED%8C%8C%EC%9D%BC%20%EC%83%9D%EC%84%B1-1)
- [[Github] Issue & PR Template 설정하기](https://amaran-th.github.io/Github/[Github]%20Issue%20&%20PR%20Template%20%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0/)

<br>
