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
2. 모달의 내용은 동적으로 변경할 수 있어야 한다.
3. 모달이 열려 있을 때, 배경 클릭 시 모달이 닫혀야 한다.

<br>

# 2. 구현하기

![](/assets/images/2024/2024-08-02-22-37-57.png)

## 2.1 App.jsx

```jsx
import Modal from "./Modal";

function App() {
  return (
    <div>
      <h3>useContext로 만드는 Modal</h3>
      <Modal />
    </div>
  );
}

export default App;
```

<br>

## 2.2 ModalContext 설정

모달의 상태를 관리하기 위해 ModalContext를 설정한다. useContext와 useState를 활용하여 모달의 열림/닫힘 상태와 모달 내용을 관리한다.

```jsx
// src/context/ModalContext.jsx
import { createContext, useContext, useState } from "react";

const ModalContext = createContext();

export const useModal = () => useContext(ModalContext);

export const ModalProvider = ({ children }) => {
  const [isOpen, setIsOpen] = useState(false);
  const [modalContent, setModalContent] = useState(null);

  const openModal = (content) => {
    setModalContent(content);
    setIsOpen(true);
  };

  const closeModal = () => {
    setIsOpen(false);
    setModalContent(null);
  };

  return (
    <ModalContext.Provider
      value={{ isOpen, openModal, closeModal, modalContent }}
    >
      {children}
    </ModalContext.Provider>
  );
};
```

<br>

## 2.3 Modal 컴포넌트 구현

ModalProvider를 통해 상태를 공유한다. 버튼을 클릭하면 모달이 열리고, 배경을 클릭하거나 닫기 버튼을 클릭하면 모달이 닫힌다.

```jsx
// src/Modal.jsx
import { ModalProvider, useModal } from "./context/ModalContext";
import styled from "styled-components";

const ModalButton = () => {
  const { openModal } = useModal();
  return (
    <button onClick={() => openModal("This is the modal content!")}>
      Open Modal
    </button>
  );
};

const ModalContent = () => {
  const { isOpen, closeModal, modalContent } = useModal();

  if (!isOpen) {
    return null;
  }

  return (
    <Overlay onClick={closeModal}>
      <Content onClick={(e) => e.stopPropagation()}>
        <CloseButton onClick={closeModal}>Close</CloseButton>
        <p>{modalContent}</p>
      </Content>
    </Overlay>
  );
};

const Modal = () => {
  return (
    <ModalProvider>
      <div>
        <ModalButton />
        <ModalContent />
      </div>
    </ModalProvider>
  );
};

export default Modal;

const Overlay = styled.div`
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

const Content = styled.div`
  background: white;
  padding: 20px;
  border-radius: 5px;
  position: relative;
`;

const CloseButton = styled.button`
  position: absolute;
  top: 10px;
  right: 10px;
`;
```

<br>
