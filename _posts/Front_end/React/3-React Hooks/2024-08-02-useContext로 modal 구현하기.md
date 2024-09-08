---
title: "[React] useContext로 modal 구현하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 기능 요구사항

1. 모달을 열고 닫을 수 있어야 한다.
2. 모달이 열려 있을 때, 배경 클릭 시 모달이 닫혀야 한다.

<br>

# 2. 구현하기

![](/assets/images/2024/2024-08-12-16-50-38.png)

## 2.1 App.jsx

```jsx
import Modal from "components/ContextPractice/Modal";
import { ModalContextProvider } from "context/ModalContext";

const HomePage = () => {
  return (
    <ModalContextProvider>
      <h1>useContext로 Modal 구현하기</h1>
      <Modal />
    </ModalContextProvider>
  );
};

export default HomePage;
```

<br>

## 2.2 ModalContext 설정

모달의 상태를 관리하기 위해 ModalContext를 설정한다. useContext와 useState를 활용하여 모달의 열림/닫힘 상태와 모달 내용을 관리한다.

{% raw %}

```jsx
// src/context/ModalContext.jsx
import { createContext, useState } from "react";

export const ModalContext = createContext();

export const ModalContextProvider = ({ children }) => {
  const [isOpen, setIsOpen] = useState(false);
  const openModal = () => {
    setIsOpen(true);
  };

  const closeModal = () => {
    setIsOpen(false);
  };
  return (
    <ModalContext.Provider value={{ isOpen, setIsOpen, openModal, closeModal }}>
      {children}
    </ModalContext.Provider>
  );
};
```

{% endraw %}

<br>

## 2.3 Modal 컴포넌트 구현

ModalProvider를 통해 상태를 공유한다. 버튼을 클릭하면 모달이 열리고, 배경을 클릭하거나 닫기 버튼을 클릭하면 모달이 닫힌다.

```jsx
// src/Modal.jsx
import { ModalContext } from "context/ModalContext";
import { useContext } from "react";
import styled from "styled-components";

const Modal = () => {
  const { openModal, isOpen, closeModal } = useContext(ModalContext);

  return (
    <div>
      <button onClick={openModal}>Open Modal</button>
      {isOpen && (
        <StOverlay onClick={closeModal}>
          <StModalBox onClick={(e) => e.stopPropagation()}>
            <button onClick={closeModal}>Close</button>
            <p>모달 열림!!✨</p>
          </StModalBox>
        </StOverlay>
      )}
    </div>
  );
};

export default Modal;

const StOverlay = styled.div`
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
`;

const StModalBox = styled.div`
  border: 2px solid black;
  width: 300px;
  height: 100px;
  background: white;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  position: relative;
`;
```

> stopPropagation : 이벤트 전파 방지

- 이벤트는 DOM 트리에서 자식 요소에서 부모 요소로, 혹은 다른 방향으로 전파된다. 이 전파를 "버블링"이라고 하며, 부모 요소가 자식 요소의 이벤트를 감지할 수 있다.
- `stopPropagation` 메서드를 호출하면, 이벤트가 더 이상 상위 요소로 전파되지 않기 때문에, 이벤트가 상위 요소에서 처리되지 않게 된다.

<br>

- 위 코드에서 모달의 Overlay가 클릭되면 모달이 닫히도록 설정한 경우, 모달 박스(StModalBox)를 클릭할 때도 Overlay가 클릭된 것으로 간주되어 모달이 닫히게 된다.
- stopPropagation을 사용하면, 모달 박스 내에서의 클릭 이벤트는 Overlay로 전파되지 않아 모달이 닫히지 않게 된다.

<br><br>

# 3. 여러 모달 관리하기

## 3.1 ModalContext.jsx

> 각 모달의 ID를 사용하여 상태를 관리할 수 있도록 `ModalContext`를 수정한다. 이를 통해 특정 모달만 열거나 닫을 수 있다.

{% raw %}

```jsx
// src/context/ModalContext.jsx
import { createContext, useContext, useState } from "react";

const ModalContext = createContext();

export const ModalProvider = ({ children }) => {
  const [openModalId, setOpenModalId] = useState(null);

  // 모달 열기
  const openModal = (id) => {
    setOpenModalId(id);
  };

  // 모달 닫기
  const closeModal = () => {
    setOpenModalId(null);
  };

  // 특정 모달이 열려 있는지 확인
  const isOpen = (id) => openModalId === id;

  return (
    <ModalContext.Provider value={{ openModal, closeModal, isOpen }}>
      {children}
    </ModalContext.Provider>
  );
};

export const useModal = () => useContext(ModalContext);
```

{% endraw %}

<br>

## 3.2 LetterCradList.jsx

> 버튼 클릭 시 모달 ID를 전달하도록 한다.

{% raw %}

```jsx
// src/Modal.jsx
const LetterCradList = ({ id }) => {
  const { isOpen, openModal } = useModal();

  return (
    <StLetterCradListContainer>
      <div>
        <ul>
          {letters.map((letter) => (
            <li key={letter.id}>
              <p>{letter.title}</p>
              <button onClick={() => openModal(letter.id)}>자세히보기</button>
              {isOpen(letter.id) && <LetterCardModal letter={letter} />}
            </li>
          ))}
        </ul>
      </div>
    </StLetterCradListContainer>
  );
};

export default LetterCradList;
```

{% endraw %}

<br>

## 3.3 LetterCardModal.jsx

> 각 모달의 고유 ID를 기반으로 열고 닫는 기능을 구현한다.

{% raw %}

```jsx
export const LetterCardModal = ({ letter }) => {
  const { closeModal } = useModal();
  const [editMode, setEditMode] = useState(null);

  return (
    <div>
      <StOverlay onClick={closeModal}>
        <StModalBox onClick={(e) => e.stopPropagation()}>
          <button onClick={closeModal}>x</button>
          {editMode ? (
            <>
              <input
                value={editMode.title}
                onChange={(e) =>
                  setEditMode({ ...editMode, title: e.target.value })
                }
              />
              <textarea
                value={editMode.content}
                onChange={(e) =>
                  setEditMode({ ...editMode, content: e.target.value })
                }
              />
              <button onClick={() => setEditMode(null)}>취소</button>
              <button onClick={() => handleEditButton(letter.id)}>
                수정 완료
              </button>
            </>
          ) : (
            <>
              <p>{letter.title}</p>
              <p>{letter.content}</p>
              <button onClick={handleDeleteButton}>삭제</button>
              <button onClick={() => handleEditMode(letter)}>수정</button>
            </>
          )}
        </StModalBox>
      </StOverlay>
    </div>
  );
};
```

<br>
