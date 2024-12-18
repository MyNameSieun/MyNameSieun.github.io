---
title: "[Python] 인코딩(Encoding)과 디코딩(Decoding)"
categories: [Python]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 인코딩(Encoding)

> 문자를 컴퓨터가 이해할 수 있는 바이트(byte) 형태로 변환하는 과정이다.

- 바이트 형태로 변환하여 파일 출력(write), 네트워크 전송(send)시에 사용한다.
- `ord(char)` : 문자의 유니코드 코드 포인트를 반환
- `chr(code)` : 코드 포인트를 문자로 변환
- `encode(encoding)` : 문자열을 특정 인코딩(utf-8, utf-16 등)으로 바이트 객체로 변환

```python
list = ["a", "가"]
for a in list:
    print(f"{a} => {hex(ord(a))} => {chr(ord(a))}")
    print(f"type(org) = {type(a)}, org = {a}")

    b = a.encode("utf-8")
    print(f"type(enc) = {type(b)}, enc={b}")
```

1. list에 있는 문자 "a"와 "가"를 하나씩 처리한다.
2. ord(a)로 문자의 유니코드 값(코드 포인트)을 확인한다.
   - "a"는 0x61 (16진수), "가"는 0xac00이다.
3. chr(ord(a))로 다시 원래 문자로 변환한다.
4. a.encode("utf-8")로 해당 문자를 UTF-8 방식으로 바이트로 변환한다.
   - "a"는 `b'a'`, "가"는 `b'\xea\xb0\x80'`.

![](/assets/images/2024/2024-12-18-14-42-17.png)

<br><br>

# 2. Decoding

> 바이트 데이터를 사람이 읽을 수 있는 문자열로 변환하는 과정이다.

파이썬에서 문자열을 입력(read)시, 네트워크에서 문자열을 수신(receive)시에 사용한다.

```python
list = ["a", "가"]

for a in list:
    print(f"{a} => {hex(ord(a))} => {chr(ord(a))}")
    print(f"type(org) = {type(a)}, org = {a}")

    b = a.encode("utf-8")
    print(f"type(enc) = {type(b)}, enc = {b}")

    c = b.decode("utf-8")
    print(f"type(dec) = {type(c)}, enc={c}")
```

1. 인코딩한 바이트 객체 b를 decode("utf-8")로 다시 문자열로 변환한다.
2. 결과적으로 원래 문자열 a를 복원하게 된다.

![](/assets/images/2024/2024-12-18-14-41-48.png)

<br><br>

# 3. 차이점 정리

| 과정         | 역할                 | 결과 예시                  |
| ------------ | -------------------- | -------------------------- |
| **Encoding** | 문자열 → 바이트 변환 | `"가"` → `b'\xea\xb0\x80'` |
| **Decoding** | 바이트 → 문자열 변환 | `b'\xea\xb0\x80'` → `"가"` |

<br>
