---
title: "[React] 클래스형 컴포넌트의 LifeCycle"
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

# 1. LifeCycle 개요

## 1.1 LifeCycle 개념

리액트 컴포넌트는 각각 [Mount] → [Update] → [Unmount]의 과정을 거친다. 사람처럼 태어나고, 변화하고 죽는 것이다.

리액트 생명주기(LifeCycle)란, 컴포넌트 중심 라이브러리의 집합체이다.

모든 컴포넌트에는 각각의 생명주기가 존재하고 각 생명주기에 맞는 메서드들이 존재한다.

![](/assets/images/2024/2024-01-29-10-46-06.png)

<br><br>

# 2. Mount

컴포넌트가 생성될 시점을 말한다.

| 메서드                                           | 설명                                                                                                                                    |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| `constructor()`                                  | 컴포넌트가 맨 처음 만들어 질 때 호출, 생성자                                                                                            |
| `getDerivedStateFromProps(nextProps, prevState)` | ① 부모 컴포넌트로부터 props를 전달받을 때, state에 값을 일치시키는 역할을 하는 메서드.<br>마운트 될 때, 업데이트(리렌더링) 될 때도 호출 |
| `render()`                                       | ① 최초 mount가 준비완료 되면 호출되는, 즉 렌더링 하는 메서드.<br>② 컴포넌트를 DOM에 마운트하기 위해 사용                                |
| `componentDidMount()`                            | 컴포넌트가 브라우저에 표시가 된 후 호출되는 메서드                                                                                      |

<br><br>

# 3. Update

컴포넌트가 갱신될 시점을 말한다.

| 메서드                     | 설명                                                                                                                                                                                        |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `getDerivedStateFromProps` | ① Mount 과정에서도 동일하게 호출되었던 메서드.<br>② 부모 컴포넌트로부터 props를 전달받을 때, state에 값을 일치시키는 역할을 하는 메서드                                                     |
| `shouldComponentUpdate`    | ① 리렌더링 여부 판단(함수 호출 결과: true / false).<br>true인 경우: 리렌더링 진행, false인 경우: 리렌더링 하지 않음.<br>② 함수형 컴포넌트에서 memo, useMemo, useCallback이 역할을 대신한다. |
| `render`                   | ① 변경사항 반영이 다 되어 준비완료 되면 호출되는, 즉 렌더링 하는 메서드.<br>② 컴포넌트를 DOM에 마운트하기 위해 사용                                                                         |
| `getSnapshotBeforeUpdate`  | ① 컴포넌트에 변화가 일어나기 직전 DOM의 상태를 저장.<br>② componentDidUpdate 함수에서 사용하기 위한 스냅샷 형태의 데이터                                                                    |
| `componentDidUpdate`       | 컴포넌트 업데이트 작업 완료 후 호출                                                                                                                                                         |

<br><br>

# 4. Unmount

컴포넌트가 DOM에서 제거되는 시점을 말한다.
| 메서드 | 설명 |
| --------------------- | --------------------------------------- |
| `componentWillUnmount` | ① 컴포넌트가 사라지기 전 호출되는 메서드<br>② useEffect의 return과 동일 |

<br>
