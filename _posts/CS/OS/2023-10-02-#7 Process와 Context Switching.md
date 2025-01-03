---
title: "[OS] #7 Process와 Context Switching"
categories: [OS]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. Process 개요

## 1.1 Process란?

> Process란?

- 실행 중인 프로그램을 말한다.
- 프로그램이 메모리에 적재하고 실행되는 순간 프로세스가 된다.<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/bf4f8b88-a824-4293-9220-3ad6f5d0d3dd/image.jpg)

  ① 실행 가능한 파일이 메모리에 올라가면 프로세스가 된다.

  ② 해당 방향으로 동적 증가

  ③ 힙 영역과 스택 영역의 크기는 가변적이다. (동적 할당 영역)

  - 힙 영역: 낮은 주소 -> 높은 주소<br>
  - 스택 영역: 높은 주소 -> 낮은 주소

<br>

- **stack**
  - 데이터를 일시적으로 저장
  - ex) 함수 매개변수, 지역변수, 반환주소 등
- **heap**
  - 프로그래머가 직접 할당할 수 있는 저장공간 (메모리 동적 할당 가능)
  - 메모리로 할당했다면 반환해야 함. 반환하지 않는다면 메모리 누수가 발생하기 때문
  - ex) 모든 자바 객체 (new 연산자로 생성), C의 Malloc()
- **data**
  - 프로그램이 실행되는 동안 유지할 데이터 저장
  - ex) 전역변수
- **text (code)**
  - 실행할 수 있는 코드, 기계어로 이뤄진 명령어가 저장
  - 읽기 전용 공간

<br>

## 1.2 Process state⭐

![](https://velog.velcdn.com/images/sieunpark/post/7ae21855-faaa-47f6-b50c-2f5fa40f5a1b/image.jpg)

① new ( 생성 ) : 프로세스가 메모리에 적재되어 이제 막 PCB를 할당받은 상태

② ready ( 준비 ) : 실행 상태로 갈 수 있는 <span style="color:indianred">유일한</span> 상태, CPU를 할당 받기를 기다림

③ running ( 실행 ) : 프로세스가 CPU를 할당받아 실행중인 상태

- 타이머 인터럽트 발생시(할당된 시간 모두 사용시) -> ready 상태
- 실행 도중 I/O 작업이나 이벤트 발생시, 기다림 -> waiting 상태

④ waiting ( 대기 ) : I/O 작업이나 Event가 발생할 때까지 대기하는 상태

⑤ terminated ( 종료 ) : 프로세스가 종료된 상태, PCB와 메모리 영역 정리

<br><br>

# 2. Queue

## 2.1 Ready Queue

> 메인 메모리에서 running 상태로 가기 위해 기다리는 큐를 말한다.

- 프로세스는 끝으로 들어온다.
- 다음으로 실행할 확률이 가장 높은 프로세스는 head에 존재한다.
- 프로세스가 실행이 끝나면 ready로 이동한다.
- <span style="color:indianred">연결 리스트</span> 구조를 갖는다. (PCB의 포인터만 바꿔주면 되기 때문임)<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/3e07a881-782b-4e3a-892e-d99699982390/image.jpg)

<br>

> 그 외에도 스케줄링 큐에는 Job Queue, Device Queue 가 있다.

- **Job Queue**
  - 모든 프로세스에 대한 큐
- **Device Queue**
  - I/O 장치에 대한 큐

<br>

## 2.2 Queueing diagram

> Queueing diagram을 살펴보자!

![](https://velog.velcdn.com/images/sieunpark/post/7d1d7e41-e9dd-4b41-9ec0-507411299cc1/image.jpg)

① 새로운 프로세스는 초기에 ready queue에 놓인다.
실행을 위해 선택(dispatch) 될 때까지 ready queue에서 기다린다.

② 프로세스가 위 4개(I/O요청, 할당시간 만료...)에 대한 이벤트 처리를 끝내면 다시 ready queue로 돌아온다. -> PCB 반납

<br><br>

# 3. Process scheduler

## 3.1 Process scheduler란?

> ready 상태인 여러 프로세스 중에서 다음 실행할 프로세스를 선택하고 CPU에 할당하는 것을 말한다.

이러한 할당 과정을 <span style="color:indianred">dispatch</span>라고 한다.

<br>

## 3.2 PCB(Process Control Block)

> PCB는 프로세스의 정보를 담고 있는 자료 구조이다.

- OS는 프로세스의 상태를 PCB에 기록해 관리한다.
- 빠르게 번갈아 수행되는 프로세스들을 관리해준다.
- 프로세스 생성 시 커널 영역에 생성, 종료시 폐기된다.

<br>

![](https://velog.velcdn.com/images/sieunpark/post/abb48bb5-9fda-445d-bb67-b42263e857b3/image.jpg)

① 프로세스 P₀ 실행

② 인터럽트 or 시스템 콜 발생시 P₀ 정보 PCB에 저장

③ PCB₁에 저장된 P₁정보 로드

④ 프로세스 P₁실행

<br><br>

# 4. Context Switching

## 4.1 Context (문맥)

> 하나의 프로세스 수행을 재개하기 위해 기억해야 할 정보를 말한다.

하나의 프로세스 문맥은 해당 프로세스 PCB에 표현되어 있다.

<br>

## 4.2 Context Switching (문맥 교환)

> Context Switching(문맥 교환)은 프로세스의 상태 정보를 교환하는 작업을 말한다.

즉, CPU가 다른 프로세스로 전환하기 위해 현재의 프로세스의 문맥을 PCB에 <span style="color:indianred">저장(백업)</span>한 후, 전환하려고 하는 새로운 프로세스의 문맥을 PCB로부터 <span style="color:indianred">로딩</span>하는 과정을 말한다.

<br>

## 4.3 문맥교환 오버헤드

> 문맥교환이 수행되는 동안 idle 상태에 빠지는 것을 말한다.

문맥 교환 동안은 일을 안 하는 것과 같다.

![](https://velog.velcdn.com/images/sieunpark/post/fa4072f0-c299-4596-9f0d-78f290725fc9/image.jpg)

<br>

## 4.4 Context Switching 원리

> 문맥 교환은 H/W를 통해 수행된다. S/W보다 H/W가 더 빠르기 때문이다.

- ⭐ 문맥 교환은 현행 레지스터 집합에 대한 <span style="color:indianred">포인터를 변경</span>한다.
- 문맥 교환 덕분에 여러 프로세스가 끊임없이 빠르게 번갈아가며 실행될 수 있다. (멀티 프로그래밍 효과)<br><br>
  ![](https://velog.velcdn.com/images/sieunpark/post/33ed7732-1325-499d-b92c-0734dae3ba36/image.jpg)

<br>
