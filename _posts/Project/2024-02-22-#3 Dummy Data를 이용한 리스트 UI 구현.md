---
title: "[React P.J, ForYou] Dummy Data를 이용한 리스트 UI 구현 #3"
categories: [Project]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Dummy Data를 이용한 리스트 UI 구현

> Dummy Data(fakeData.json)를 이용한 리스트 UI 구현해보자!

## 1.1 fakeData.json 생성

먼저 fakeData.json를 생성하고 아래 내용을 넣어주자.

```json
// src/fakeData.json
[
  {
    "createdAt": "2023-11-03T02:07:09.423Z",
    "nickname": "시은",
    "avatar": "https://blog.kakaocdn.net/dn/lbo8w/btqUuM1uBuS/JnwpyOcvjWuQHnlJ5jBCNK/img.jpg",
    "content": "안녕하세요 토토로! 언제나 행복한 일만 가득하길 바라요. 당신은 정말 특별하고 사랑스러운 존재에요. 언제나 행운과 기쁨이 당신과 함께 하길 기원합니다! 🌟",
    "writedTo": "토토로",
    "id": "1"
  },
  {
    "createdAt": "2023-11-02T23:13:18.491Z",
    "nickname": "현우",
    "avatar": "https://mblogthumb-phinf.pstatic.net/MjAxNzAyMjVfMTE4/MDAxNDg3OTc0OTA1NDY1.WUn5KGFrnQNzu7R91G7QeICVHbkLQb3qJXhlk19PFLYg.mkQ1ZNBE_fF4bTZTimCMsUvLxB-DfrU1q1v_amc_vm8g.JPEG.jkh6564/%EB%B2%BC%EB%9E%91%EC%9C%84%EC%9D%98%ED%8F%AC%EB%87%A8.jpg?type=w800",
    "content": "안녕하세요 포뇨! 당신은 마법과 모험으로 가득한 멋진 세계에서 왔죠. 항상 당신의 용기와 순수함이 많은 이들에게 영감을 주고 있어요. 언제나 행복하고 안전하게 모험을 즐기길 바라며, 당신의 모든 꿈이 이루어지기를 기대합니다! 🌈✨",
    "writedTo": "포뇨",
    "id": "2"
  },
  {
    "createdAt": "2023-11-02T11:25:37.026Z",
    "nickname": "지훈",
    "avatar": "https://ojsfile.ohmynews.com/STD_IMG_FILE/2016/0304/IE001931016_STD.jpg",
    "content": "안녕하세요 가오나시! 당신은 신비로운 모습과 특별한 능력으로 많은 이들에게 깊은 인상을 남기고 있어요. 언제나 당신의 곁에는 신기한 모험이 기다리고 있겠죠. 항상 건강하고 행복하게 모험을 즐기길 바라며, 당신의 모든 순간이 특별하고 의미있기를 기원합니다! 🌟🦊",
    "writedTo": "가오나시",
    "id": "3"
  },
  {
    "createdAt": "2023-11-02T16:06:34.150Z",
    "nickname": "민지",
    "avatar": "https://i.namu.wiki/i/ja3KaNWwMqRU1PtwyLI2mZG8zS9rfFawEkR_Wh2-aucLIMeZaf-bz_QuU2D8wQaaI7ztXB8rCGvUmyoEINgVBA.webp",
    "content": "안녕하세요 키키! 당신은 항상 재미와 웃음으로 가득 찬 모습으로 주변을 밝게 만들어주고 있어요. 당신의 긍정적인 에너지는 마치 햇살처럼 모두를 따뜻하게 만듭니다. 항상 행복하고 유쾌한 순간들이 당신을 따라다니길 바라며, 미소가 결코 떨어지지 않기를 기원합니다! 😄🌈",
    "writedTo": "키키",
    "id": "4"
  },
  {
    "createdAt": "2023-11-03T05:40:17.575Z",
    "nickname": "서연",
    "avatar": "https://i.namu.wiki/i/RJfXvBj--BauQbtJsUZ5c0OHyO1B9aE0lkhA6NYd-dlAh9XrUJRHMt6-OY5KBeEd5wRw3LUnhae6OY29SWtEgw.webp",
    "content": "안녕하세요 치히로! 당신은 용기와 결단력으로 신비로운 세계에서 여러 어려움을 극복해 나가는 모습이 정말 멋지고 감동적이에요. 언제나 당신의 모험이 행복과 성취감으로 가득하길 바라며, 미래의 모든 모험에서도 힘과 용기를 유지하길 기원합니다! 🚂🌟",
    "writedTo": "치히로",
    "id": "5"
  }
]
```

<br>

## 1.2 json import

json 파일은 별도로 export 하지 않아도 import를 할 수 있다.

```js
import fakeData from "fakeData.json"; // import

function LetterList() {
  console.log("fakeData", fakeData);

  <LetterCard />
  <LetterCard />
  <LetterCard />
}
```

import를 한 뒤, console.log로 확인하면, json 파일을 배열로 잘 받아 온 것을 확인할 수 있다.

![](/assets/images/2024/2024-02-22-12-18-40.png)

<br>

## 1.3 트러블 슈팅💫

> 발생한 문제 🤦‍♀️

이제 기존 코드를 Dummy Data로 교체하기 위해 아래와 같이 작성해줬더니 출력이 되지 않는 문제가 있었다.

```js
// LetterList.jsx
import fakeData from "fakeData.json"; // import

function LetterList() {
  return (
    <LetterContainer>
      {fakeData.map((letter, index) => {
        <LetterCard letter={letter} key={index} />;
      })}
    </LetterContainer>
  );
}
```

<br>

> 해결 방안(1) 💁‍♀️

React에서는 화살표 함수에서 중괄호를 사용하면 명시적으로 return문을 선언해야한다는데 retrun문을 사용하지 않아 발생한 문제였다. 따라서 retrun문을 선언해주었다.

```js
// LetterList.jsx
import fakeData from "fakeData.json"; // import

function LetterList() {
  return (
    <LetterContainer>
      {fakeData.map((letter, index) => {
        return <LetterCard letter={letter} key={index} />;
      })}
    </LetterContainer>
  );
}
```

<br>

> 해결 방안(2) 💁‍♀️

화살표 함수는 한 줄로 작성된 경우에 return문을 생략할 수 있어서, 중괄호와 return문을 둘 다 사용하지 않도록 하였다.

```js
// LetterList.jsx
import fakeData from "fakeData.json"; // import

function LetterList() {
  return (
    <LetterContainer>
      {fakeData.map((letter) => (
        <LetterCard letter={letter} />
      ))}
    </LetterContainer>
  );
}
```

json에 들어있는 데이터만큼 잘 출력이 된다!

![](/assets/images/2024/2024-02-22-12-05-15.png)

<br>

## 1.4 데이터 넣어주기

> LettetList에 props으로 전달받은 json 데이터를 활용해 데이터를 넣어주자!

기존 코드

```js
function LetterCard() {
  return (
    <>
      <LetterAvatarFigure>
        <img src={null ?? defaultUser} alt="아바타이미지" />
      </LetterAvatarFigure>
      <LetterBox>
        <LetterDate>
          <p>닉네임</p>
          <time>2023.01.31 오전 11:15</time>
        </LetterDate>
        <LetterButtonBox>
          <LetterContent>
            <p>내용</p>
          </LetterContent>
          <LetterButton>자세히보기</LetterButton>
        </LetterButtonBox>
      </LetterBox>
      <Hr />
    </>
  );
}
```

```js
// LetterCard/jsx
function LetterCard({ letter }) {
  return (
    <>
      <LetterAvatarFigure>
        <img src={letter.avatar ?? defaultUser} alt="아바타이미지" />
      </LetterAvatarFigure>
      <LetterBox>
        <LetterDate>
          <p>{letter.nickname}</p>
          <time>{letter.createdAt}</time>
        </LetterDate>
        <LetterButtonBox>
          <LetterContent>
            <p>{letter.content}</p>
          </LetterContent>
          <LetterButton>자세히보기</LetterButton>
        </LetterButtonBox>
      </LetterBox>
      <Hr />
    </>
  );
}
```

![](/assets/images/2024/2024-02-22-12-28-52.png)

<br>

### 1.4.1 날짜 형식 바꾸기

`"createdAt": "2023-11-03T02:07:09.423Z"` 의 날짜 형식을 `23.11.03. 오전 02:07:09` 이렇게 바꿔보자.

```js
function LetterCard({ letter }) {
  const formattedDate = new Date(letter.createdAt).toLocaleDateString("ko", {
    year: "2-digit",
    month: "2-digit",
    day: "2-digit",
    hour: "2-digit",
    minute: "2-digit",
    second: "2-digit",
  });

  return (
    <>
      <LetterAvatarFigure>
        <img src={letter.avatar ?? defaultUser} alt="아바타이미지" />
      </LetterAvatarFigure>
      <LetterBox>
        <LetterDate>
          <p>{letter.nickname}</p>
          <time>{formattedDate}</time>
        </LetterDate>
        <LetterButtonBox>
          <LetterContent>
            <p>{letter.content}</p>
          </LetterContent>
          <LetterButton>자세히보기</LetterButton>
        </LetterButtonBox>
      </LetterBox>
      <Hr />
    </>
  );
}
```

![](/assets/images/2024/2024-02-22-12-41-58.png)

<br>

### 1.4.2 공통 함수 만들기

> 위에서 만든 날짜 형식을 다른 컴포넌트에서도 사용할 수 있게 공통 함수로 만들어주자

src/util/data.js에 아래와 같이 만들어주자

```js
// src/util/data.js
export const getFormattedDate = (date) =>
  new Date(date).toLocaleDateString("ko", {
    year: "2-digit",
    month: "2-digit",
    day: "2-digit",
    hour: "2-digit",
    minute: "2-digit",
    second: "2-digit",
  });
```

<br>

LetterCard에 생성한 getFormattedDate 함수 사용하기!<br>
createdAt을 인자로 넣어줘야한다.

```js
//  LetterCard.js
...
import { getFormattedDate } from "util/data"; // import

function LetterCard({ letter }) {
  const navigate = useNavigate();

  return (
    <>
      <LetterDate>
        <p>{letter.nickname}</p>
        <time>{getFormattedDate(letter.createdAt)}</time>
      </LetterDate>
    </>
  );
}
```

<br>

## 1.4.2 텍스트 한 줄만 나오게 하기

편지 내용은 딱 한 줄까지만 표현하고 한 줄이상의 컨텐츠일 경우 … 으로 표현해보자!

```css
& p {
  white-space: nowrap; // 줄바꿈x
  overflow: hidden;
  text-overflow: ellipsis; // 생략 부호(...)
}
```

![](/assets/images/2024/2024-02-22-12-40-54.png)

<br>

## 1.4.3 tab filter 적용

> 탭 클릭 시 해당 캐릭터에게 쓰여진 편지 리스트만 보이게 구현해보자.

Home 컴포넌트에서 activeTab과 setActiveTab을 정의하여, Tabs와 LetterList 컴포넌트에 props로 전달하였다.
(원래는 Tabs 컴포넌트에 있었는데 상위로 올려준 것 → Props Drilling)

```js
// Home.jsx
function Home() {
  const [activeTab, setActiveTab] = useState("토토로");

  return (
    <HomeLayout>
      <Navbar />
      <HomeRow>
        <Tabs activeTab={activeTab} setActiveTab={setActiveTab} />
        <HomeCol>
          <AddForm />
          <LetterList activeTab={activeTab} />
        </HomeCol>
      </HomeRow>
    </HomeLayout>
  );
}
```

activeTab prop을 받아 해당 캐릭터에게 쓰여진 편지 리스트를 필터링한다.

```js
// LetterList.jsx
function LetterList({ activeTab }) {
  const filteredLetters = fakeData.filter(
    (letter) => letter.writedTo === activeTab
  );
  return (
    <LetterContainer>
      {filteredLetters.map((letter, index) => (
        <LetterCard letter={letter} key={index} />
      ))}
    </LetterContainer>
  );
}
```

![](/assets/images/2024/2024-02-22-13-09-05.png)

<br>
