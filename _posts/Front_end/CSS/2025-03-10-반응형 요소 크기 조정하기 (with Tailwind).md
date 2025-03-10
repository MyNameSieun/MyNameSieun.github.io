---
title: "[CSS] 반응형 요소 크기 조정하기 (with Tailwind)"
categories: [CSS]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

> 구현 목표

Tailwind CSS를 사용하여 부모 요소의 크기에 따라 자동으로 조정되는 UI 요소를 구현해보자

<br>

> 예시 코드

부모 요소의 크기에 맞춰 이미지와 흰색 바의 크기가 자동으로 조정되도록 Tailwind CSS의 반응형 유틸리티(`w-full`, `h-auto`, `w-[calc(100%)]`)를 활용한 배너 컴포넌트

```tsx
const SignPageBanner = () => {
  return (
    <section className="w-full h-full flex justify-center items-center">
      <Image
        src="/images/icons/idea.png"
        alt="idea-icon"
        width={600}
        height={600}
        className="z-10 relative w-full h-auto"
      />
      <div className="w-[calc(100%)] h-6 bg-white absolute top-1/2 left-0 rounded-3xl shadow-lg z-5" />
    </section>
  );
};

export default SignPageBanner;
```

<br>

- 부모 요소 크기에 맞춰 자동 조정
  - `w-full h-auto`를 사용하여 자식 요소가 부모 요소의 너비에 맞춰 크기를 조정하도록 한다.
- 오버레이 요소 자동 크기 조정
  - `w-[calc(100%)]`을 사용하여 흰색 바(div)가 부모 요소의 크기에 맞춰 줄어들도록 설정한다.
- Tailwind CSS의 반응형 유틸리티 사용
  - `max-w-full`, `max-h-full` 등의 유틸리티를 사용하면 자식 요소가 부모 요소를 넘지 않도록 설정할 수 있다.
- 유연한 레이아웃 조정
  - `flex-grow`, `flex-shrink`, `basis-auto` 등의 유틸리티 클래스를 활용하면 자식 요소들이 부모 요소의 크기에 맞춰 자연스럽게 크기가 변하게 할 수 있다.

<br>
