---
title: "[TIL] useState와 useRef를 사용하는 input 내부의 값 관리"
categories: [TIL]
tag: [TIL]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

## input 내부의 값 관리를 하려면 useState와 useRef중 어떤 것을 사용해야 효율적일까?

![](/assets/images/2024/2024-02-01-02-50-04.png)

<br>

> 우선 useState와 useRef의 차이에 대해서 알아보자.

| Hook                  | useState                              | useRef                                                        |
| --------------------- | ------------------------------------- | ------------------------------------------------------------- |
| 저장 공간 및 리렌더링 | 값 변경 → 리렌더링 → 내부 변수 초기화 | 값 변경 → 리렌더링x → 변수 값 유지( unmount 전까지 값을 유지) |
| DOM 접근              | 리렌더링 시에도 접근 가능             | 리렌더링 시 특정 요소에 바로 접근                             |
| 리렌더링 유발         | 값 변경 시 리렌더링 발생              | 값 변경 시 리렌더링 미발생                                    |

<br>

정리하자면,

- useState는 리렌더링이 꼭 필요한 값을 다룰 때 쓰면 된다.
- ref는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.<br>
  즉, 변화는 감지하지만 변화가 렌더링을 발생시키면 안되는 값을 다룰 때 사용하는 것이다.

<br>

따라서,

- useState를 사용하여 input 내부의 값과 관련된 상태를 업데이트할 수 있다.
- useRef를 사용하여 이전 값과 현재 값의 변화를 비교하거나, 특정 조건에 따라 DOM 요소를 조작하는 등의 작업을 수행할 수
  있다.

<br>

> useState와 useRef의 차이점을 코드를 통해 살펴보자!

```jsx
import "./App.css";
import { useRef, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const countRef = useRef(0);

  const HandlePlusStateBtn = () => {
    setCount(count + 1);
  };

  const HandlePlusRefBtn = () => {
    countRef.current++;
  };

  return (
    <>
      <div>
        state: {count} <br />
        <button onClick={HandlePlusStateBtn}>state 증가</button>
      </div>
      <div>
        ref: {countRef.current} <br />
        <button onClick={HandlePlusRefBtn}>ref 증가</button>
      </div>
    </>
  );
}

export default App;
```

ref와 달리 state는 컴포넌트 내에서 값이 변경되어도 렌더링이 되지 않는다는 차이때문에 `HandlePlusStateBtn`와 `HandlePlusRefBtn` 함수는 다르게 동작한다.

버튼 클릭시 state와 ref는 둘 다 증가하지만, ref는 화면에 렌더링이 되지 않을 뿐이다. 따라서 콘솔창에는 정상 출력된다.

<br>

> useState와 useRef 중 input 내부의 값 관리를 하려면 어떤 것이 더 효율적일까?

**useState가 더 효율적일 것 같다고 생각한다.**

왜냐하면 useState는 상태가 변경될 때마다 컴포넌트를 리렌더링하기 때문에 input 값이 변경될 때마다 "화면에 실시간으로 반영되게 할 수 있기 때문" 이다.

![](/assets/images/2024/2024-02-01-03-49-14.png)

<br>

팀원분은 사용자가 입력한 내용에 따라 "연관 검색어를 실시간으로 상태를 업데이트 할 수 있기 때문"에 useState가 더 효율적일 것이라 생각한다고 하셨다. 나도 이에 동감하는 바이다.

<br><br>

🙎‍♀️**만약 사용자가 입력한 내용을 실시간으로 확인하지 못한다면 어떻게 될까?**<br>
아마도 사용자는 정확한 데이터를 입력하지 못할 것이다. 사용자가 입력한 내용이 화면에 출력되지 않아 무엇을 입력했는지 알 수 없기 때문이다.

이는 사용자 경험(UX)를 크게 저해할 수 있다.

<br>

따라서 이러한 이유로 useState가 input 내부의 값 관리에 적합하다고 생각한다.

<br>
