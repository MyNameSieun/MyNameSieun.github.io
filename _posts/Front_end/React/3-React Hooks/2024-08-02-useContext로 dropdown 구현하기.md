---
title: "[React] useContext로 dropdown 구현하기"
categories: [React]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 개요

`<select>` 요소를 사용하여 드롭다운을 구현하면 커스터마이징이 어렵다. React의 useContext와 useState를 활용하면 더 유연하고 스타일링 가능한 드롭다운을 만들 수 있다.

<br>

# 2. 기능 요구사항

1. 드롭다운 버튼을 클릭하면, "첫 번째 아이템", "두 번째 아이템", "세 번째 아이템"이 나타난다.
2. 아이템 중 하나를 선택하면 버튼의 텍스트가 선택한 아이템으로 변경된다.
3. 아이템을 선택한 후, 드롭다운 메뉴가 자동으로 닫힌다.

<br>

# 3. 구현하기

![](/assets/images/2024/2024-08-02-22-43-06.png)

## 3.1 App.jsx

```jsx
import Dropdown from "./Dropdown";

const items = ["첫 번째 아이템", "두 번째 아이템", "세 번째 아이템"];

function App() {
  return (
    <div>
      <h3>useContext로 만드는 Dropdown Item</h3>
      <Dropdown items={items} />
    </div>
  );
}

export default App;
```

<br>

## 3.2 DropdownContext 설정

드롭다운의 상태를 관리하기 위해 DropdownContext를 설정한다. useContext와 useState를 활용하여 드롭다운의 열림/닫힘 상태와 선택된 아이템을 관리한다.

```jsx
// src/context/DropdownContext.jsx
import { createContext, useContext, useState } from "react";

const DropdownContext = createContext();

export const useDropdown = () => useContext(DropdownContext);

export const DropdownProvider = ({ children }) => {
  const [isOpen, setIsOpen] = useState(false);
  const [selectedItem, setSelectedItem] = useState(null);

  const toggleDropdown = () => setIsOpen(!isOpen);
  const selectItem = (item) => {
    setSelectedItem(item);
  };

  return (
    <DropdownContext.Provider
      value={{ isOpen, toggleDropdown, selectItem, selectedItem }}
    >
      {children}
    </DropdownContext.Provider>
  );
};
```

<br>

## 3.3 Dropdown 컴포넌트 구현

드롭다운 버튼과 메뉴를 구현하여 DropdownProvider를 통해 상태를 공유한다. 버튼을 클릭하면 드롭다운 메뉴가 열리고, 메뉴 아이템을 클릭하면 선택된 아이템으로 버튼 텍스트가 변경되며 드롭다운이 닫힌다.

```jsx
import { DropdownProvider, useDropdown } from "./context/DropdownContext";

const DropdownButton = () => {
  const { toggleDropdown, selectedItem } = useDropdown();
  return (
    <button onClick={toggleDropdown}>
      {selectedItem ? selectedItem : "Dropdown Button"}
    </button>
  );
};

const DropdownMenu = ({ items }) => {
  const { isOpen, toggleDropdown, selectItem } = useDropdown();

  if (!isOpen) {
    return null;
  }
  return (
    <ul>
      {items.map((item) => (
        <li
          key={item}
          onClick={() => {
            // (1) Dropdwon이 닫힘
            toggleDropdown();
            // (2) selectItem이 선택됨
            selectItem(item);
          }}
        >
          {item}
        </li>
      ))}
    </ul>
  );
};

const Dropdown = ({ items }) => {
  return (
    <DropdownProvider>
      <div>
        <DropdownButton />
        <DropdownMenu items={items} />
      </div>
    </DropdownProvider>
  );
};

export default Dropdown;
```

<br>
