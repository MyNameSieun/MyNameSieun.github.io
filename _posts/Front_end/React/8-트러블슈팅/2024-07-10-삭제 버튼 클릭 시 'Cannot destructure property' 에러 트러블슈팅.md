---
title: "[React] 삭제 버튼 클릭 시 'Cannot destructure property' 에러 트러블슈팅💫"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 발생한 문제 🤦‍♀️

삭제 로직 구현 후 삭제 버튼을 클릭했을 때 “Cannot destructure property ‘avatar’ of ‘letters.find(…)’ as it is undefined.” 이라는 에러가 출력됐다.

```jsx
function Detail({ letters, setLetters }) {
  const { id } = useParams();
  const { avatar, nickname, createdAt, content } = letters.find(
    (letter) => letter.id === id
  );

  const handleDeleteBtn = () => {
    const newLetters = letters.filter((letter) => id !== letter.id);
    setLetters(newLetters);
  };

  return (
    <DetailContainer>
      <DetailBox>
        <Link to="/">
          <DetailBackClick>x</DetailBackClick>
        </Link>
        <DetailRow>
          <Avatar src={avatar} size="large" />
          <p>{nickname}</p>
          <time>{getFormattedDate(createdAt)}</time>
        </DetailRow>
        <DetailHr />
        <DatailContent>{content}</DatailContent>
        <DetailButton>
          <button>수정</button>
          <button onClick={handleDeleteBtn}>삭제</button>
        </DetailButton>
      </DetailBox>
    </DetailContainer>
  );
}
```

<br>

# 문제 원인 🤷‍♀️

삭제 버튼 클릭시 letters.id는 삭제가 되었지만, useParams로 받아온 id는 삭제가 되지 않아 find 메서드가 일치하는 id를 찾지 못했기 때문에 find 메서드의 반환값이 undefined가 되어서, 이후에 구조 분해 할당을 하려고 할 때 에러가 발생한 것이다.

![](/assets/images/2024/2024-07-10-13-31-13.png)

<br>

# 해결 방안 💁‍♀️

- 미리 home으로 이동시킨 후 setLetters를 실행시켜주었다.
- 홈 화면으로 이동시키기 위해 useNavigate 훅을 사용하였다.

```jsx
import Avatar from "components/common/Avatar";
import styled from "styled-components";
import { Link, useNavigate, useParams } from "react-router-dom"; // import
import { getFormattedDate } from "util/data";

function Detail({ letters, setLetters }) {
  const navigate = useNavigate();
  const { id } = useParams();
  const { avatar, nickname, createdAt, content } = letters.find(
    (letter) => letter.id === id
  );

  const handleDeleteBtn = () => {
    const newLetters = letters.filter((letters) => letters.id !== id);
    navigate("/");
    setLetters(newLetters);
  };
}
```

<br>
