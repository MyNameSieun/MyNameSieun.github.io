---
title: "[React] Component 수정하기"
categories: [React]
tag: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> 아래와 같은 기능을 구현해보자.

1. 수정버튼 눌렀을 때, content 부분(p 태그)을 textarea로 변경하여 수정 중인 상태 나타내기
2. 수정 버튼을 취소 버튼으로 변경, 삭제 버튼을 수정 완료 버튼으로 변경
3. 수정 중인 상태인지 아닌지 알아야하기 때문에 상태값 정의

```js
const [isEditing, setIsEditing] = useState(false); // import,(수정중이 아니므로 false)

return (
  <Container>
    <DetailContainer>
      <BackClick onClick={() => navigate(-1)}>x</BackClick>
      <FlexRow>
        <UserImage src={letters.avatar} />
        <NickName>{nickname}</NickName>
        <CreatedAt>{createdAt}</CreatedAt>
      </FlexRow>
      <Hr />
      {/* {isEditing? true일 때 보일 화면 : flase일 때 보일 화면} */}
      {isEditing ? (
        <>
          {/* autoFocus로 자동포커스, defaultValue로 기존의 입력된 상태가 나오게 하기 */}
          <Textarea autoFocus defaultValue={content} />
          <Button onClick={() => setIsEditing(false)}>취소</Button>
          <Button onClick={onEditDone}>수정완료</Button>
        </>
      ) : (
        <>
          <Content>{content}</Content>
          <EditButton onClick={() => setIsEditing(true)}>수정</EditButton>
          <DeleteButton onClick={() => handleRemoveBtn(id)}>삭제</DeleteButton>
        </>
      )}
    </DetailContainer>
  </Container>
);
```

<br>

수정된 text를 가지고 있기 위한 state 정의

```js

...

const [editingText, setEditingText] = useState("");

  const onEditDone = () => {
    if (!editingText) return alert("수정사항이 없습니다.");

    // 수정 로직은 삭제하는 로직이랑 흡사
    // fakeData의 url에 들어온 id값에 따라서 content의 내용을 다른 것으로 바꾸고 나머지는 유지한 상태로 새로운 배열 반환

    const newLetters = letters.map((letter) => {
      // letters의 id가 Path parameter로 가져온 아이디와 같다면
      if (letter.id === id) {
        // 그것만 content를 editingText로 바꿔주자
        return { ...letter, content: editingText };
      }

      return letter;
    });

    setLetters(newLetters);
    setIsEditing(false);
    setEditingText("");
  };


  <Textarea autoFocus defaultValue={content} onChange={e=>setEditingText(e.target.value)}/>

...

```

<br>
