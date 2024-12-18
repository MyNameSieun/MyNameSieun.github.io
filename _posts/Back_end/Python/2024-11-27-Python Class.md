---
title: "[Python] Python Class"
categories: [Python]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

---

[[점프 투 파이썬↗️]](https://wikidocs.net/28)을 바탕으로 정리한 글입니다.
{: .notice--danger}

---

<br>

# 1. 클래스는 왜 필요할까?

클래스는 왜 필요할까? 계산기 프로그램을 만들며 확인해보자!

<br>

## 1.1 절차 지향 프로그래밍

> 절차 지향 프로그래밍에서는 데이터를 중심으로 함수와 전역 변수를 활용하여 작업을 수행한다.

그러나, 여러 개의 계산기가 필요한 상황에서는 별도의 함수와 변수를 계속 만들어야 하는 단점이 있다.

```python
# calculator.py
result = 0

def add(num):
    global result
    result+=num
    return result

print(add(3)) # 3
print(add(4)) # 7
```

만약 계산기가 2개라면 다음과 같이 관리가 복잡해집니다.

```python
# calculator2.py
result1=0
result2=0

def add(num):
    global result1
    result1 += num
    return result1

def add(num):
    global result2
    result2 += num
    return result2

print(add(3)) # 3
print(add(4)) # 7
print(add(5)) # 5
```

<br>

## 1.2 객체 지향 프로그래밍

> 객체 지향 프로그래밍(OOP)에서는 데이터를 **객체**라는 단위로 묶어 관리한다.

- 클래스는 객체를 생성하는 설계도 역할을 하며, 반복적인 변수와 메서드를 정의할 수 있다.
- 계산기를 클래스로 설계하면 여러 개의 계산기를 효율적으로 관리할 수 있습니다.

```python
# calculator3.py
class Calculator:
    def __init__(self):
        self.result=0

    def add(self, num):
        self.result += num
        return self.result

cal1 = Calculator()
cal2 = Calculator()

print(cal1.add(3))
print(cal1.add(4))
```

<br><br>

# 2. 클래스 작성 및 사용하기

## 2.1 클래스 작성하기

> 클래스는 변수와 메서드로 구성된다.

- 메서드(method): 클래스 내부에서 정의된 함수
- 메서드의 첫 번째 매개변수는 관례적으로 self를 사용하며, 호출한 객체 자신을 가리킨다.
- 즉, `a.setdata(4, 2)`처럼 호출하면 setdata 메서드의 첫 번째 매개변수 self에는 setdata 메서드를 호출한 객체 a가 자동으로 전달된다.

```python
# FourCal.py
class FourCal:
    def setData(self,first,second):
        self.first=first
        self.second=second

    def add(self):
        result=self.first+self.second
        return result

    def mul(self):
        result=self.first*self.second
        return result

    def sub(self):
        result=self.first-self.second
        return result

    def div(self):
        result=self.first/self.second
        return result
```

![](/assets/images/2024/2024-11-27-17-14-00.png)

<br>

## 2.2 클래스 사용하기

- 클래스에서 객체를 생성하여 메소드를 호출한다.
- 생성한 객체(a) 자체가 매개변수의 `self` 로 들어간다.
- 동일한 클래스로 만든 객체들은 서로 영향을 주지 않는다.

  ```python
  # fourCalClassTest.py
  from FourCal import FourCal

  a = FourCal() # 객체 생성
  a.setData(4,2) # 메서드 호출
  print(a.add()) # 6
  ```

<br><br>

# 3. 생성자(constructor)

## 3.1 생성자의 역할

> 생성자는 객체가 생성될 때 **자동으로 호출되는 메서드**를 의미한다.

주로 객체 생성 시 초기값 설정을 강제하기 위해 사용된다.

<br>

생성자를 사용하지 않으면 다음과 같은 문제가 발생할 수 있다.

```python
a = FourCal()
# 초기값 설정 누락
print(a.add())  # 오류 발생

```

<br>

## 3.2 생성자 작성

> 생성자는 `__init__` 메서드로 정의하며, 객체 생성 시 자동으로 호출된다.

객체 생성 시 초기값을 전달하여 오류를 예방할 수 있다.

```python
# FourCal.py
class FourCal:
    def __init__(self,first,second):
        self.first=first
        self.second=second

    def add(self):
        result=self.first+self.second
        return result

    def mul(self):
        result=self.first*self.second
        return result

    def sub(self):
        result=self.first-self.second
        return result

    def div(self):
        result=self.first/self.second
        return result
```

```python
# fourCalClassTest.py
from FourCal import FourCal

a=FourCal(4,2) # 객체 생성 시 초기값 설정
print(a.add()) # 6
```

![](/assets/images/2024/2024-11-27-17-53-43.png)

위와 같이 **생성 할 때부터** 값 입력을 강제하기 때문에 에러를 예방할 수 있다.

<br><br>

# 4. 클래스의 상속(Inheritance)

## 4.1 상속의 개념

> 상속은 기존 클래스를 재사용하여 새로운 클래스를 만드는 기능이다.

- 부모 클래스: 기존 클래스
- 자식 클래스: 부모 클래스를 확장한 클래스

<br>

> 상속의 목적

- 클래스가 라이브러리로 제공되어 수정이 불가할 때 사용한다.
- 이미 존재하는 클래스에 기능을 추가하거나 기존 기능을 변경하려 할 때 사용한다.
  - e.g., 이미 존재하는 계산기를 상속받아 → 공학용 계산기를 생성<br><br>
    ![](/assets/images/2024/2024-12-18-13-59-56.png)

<br>

## 4.2 상속 사용 방법

> 클래스를 상속하기 위해서는 다음처럼 클래스 이름 뒤 괄호 안에 상속할 클래스 이름을 넣어주면 된다.

```python
class 클래스_이름(상속할 클래스_이름)
# class 자식_클래스_이름(부모_클래스_이름)
```

<br>

> `a^b`를 계산하는 MoreFourCal 클래스를 만들어보자.

```python
# fourCalClassTest.py
from FourCal import FourCal

class MoreFourCal(FourCal):
    pass

a=MoreFourCal(4,2)
print(a.add()) # 6
```

<br>

## 4.3 기능 추가(메서드 확장)

> 위 코드에서 **부모 클래스의 기능에 새로운 메서드를 추가** 해보자.

pass 문장은 삭제하고 두 수의 거듭제곱을 구할 수 있는 pow 메서드를 추가했다.

```python
# fourCalClassTest.py
from FourCal import FourCal

class MoreFourCal(FourCal):
    def pow(self):
        result=self.first ** self.second
        return result

a=MoreFourCal(4,2)

print(a.add()) # 6
print(a.pow()) # 16
```

상속은 MoreFourCal 클래스처럼 기존 클래스(FourCal)는 그대로 놔둔 채 클래스의 기능을 확장할 때 주로 사용한다.

<br>

## 4.3 메소드 오버라이딩

> 부모 클래스의 메서드를 자식 클래스에서 다시 정의하는 것을 메서드 오버라이딩(method overriding)이라고 한다.

- 상속받은 기능을 자식 클래스에 맞게 변경하거나 확장하려고 사용한다.
- 자식 클래스의 메서드가 부모 클래스의 메서드를 덮어씌워서 우선적으로 실행되기 때문에, "자식 이기는 부모 없다"는 말로 비유할 수 있다.

<br>

아래 코드는 0으로 나누려고 하기 때문에 오류가 발생한다.

```python
# fourCalClassTest.py
from FourCal import FourCal

class MoreFourCal(FourCal):
    def pow(self):
        result=self.first ** self.second
        return result

a=MoreFourCal(4,0)

print(a.div()) # error
```

<br>

> SafeFourCal 클래스에 오버라이딩한 div 메서드는 나누는 값이 0인 경우에는 0을 리턴하도록 수정했다.

- 이렇게 메서드를 오버라이딩하면 부모 클래스의 메서드 대신 오버라이딩한 메서드가 호출된다.
- FourCal 클래스와 달리 ZeroDivisionError가 발생하지 않고 의도한 대로 0이 리턴되는 것을 확인할 수 있다.

```python
# fourCalClassTest.py
from FourCal import FourCal

class SafeFourCal(FourCal):
    def div(self):
        if self.second==0: # 나누는 값이 0인 경우 0을 리턴하도록 수정
            return 0
        else:
            return self.first / self.second

a=SafeFourCal(4,0)
print(a.div()) # 0
```

<br><br>

# 5. 클래스 변수

> 우리가 앞서 공부한 **객체 변수**에서는 각 객체마다 독립적으로 다른 값을 가질 수 있었다.

따라서 아래 `a = SafeFourCal(4, 0)`과 `b = SafeFourCal(3, 7)`은 서로 다른 값을 가진다.

```python
a = SafeFourCal(4,0)
b = SafeFourCal(3,7)
```

<br>

> 클래스 변수는 클래스에 속한 변수로, 모든 객체가 값을 공유한다.

- 클래스 변수는 `Family.lastname`처럼 클래스 이름으로 접근하거나, 객체를 통해 접근할 수 있다.
- 모든 객체가 값을 공유하기 때문에 클래스 변수의 값을 바꾸면 모든 객체에서 영향을 받는다.
- e.g., 회사 이름, 학과 이름 등 모든 객체가 공통으로 가져야 할 값을 정의할 때 사용한다.

```python
class Family:
    lastname = "냥"  # 클래스 변수

# 객체 생성
a = Family()
b = Family()

print(a.lastname)  # 냥
print(b.lastname)  # 냥

# 클래스 변수를 변경하면?
Family.lastname = "멍"
print(a.lastname)  # 멍
print(b.lastname)  # 멍
```

<br>
