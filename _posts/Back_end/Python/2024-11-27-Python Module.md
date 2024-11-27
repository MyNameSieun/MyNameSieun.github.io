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

[[점프 투 파이썬↗️]](https://wikidocs.net/29)을 바탕으로 정리한 글입니다.
{: .notice--danger}

- 모듈과 패키지는 사용하는 입장이지 보통 만들 일은 없기 때문에 사용자 관점에서 가볍게 공부하도록 하자.

---

<br>

# 1. 모듈

## 1.1 모듈의 정의

> 모듈이란 함수나 변수 또는 클래스를 모아 놓은 파이썬 파일이다. 모듈은 다른 파이썬 프로그램에서 불러와 사용할 수 있도록 만든 파이썬 파일이라고도 할 수 있다.

- e.g.,
  - math, sys와 같은 내장 모듈
  - 사용자 정의 모듈 (e.g., FourCal.py)

![](/assets/images/2024/2024-11-27-18-25-01.png)

<br>

## 1.2 모듈 만들기

> 모듈을 만들려면, 함수나 클래스를 정의한 파이썬 파일을 생성하면 된다.

위와 같은 파일을 만들면, 다른 파이썬 프로그램에서 이 모듈을 가져와 사용할 수 있다.

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

<br>

## 1.3 모듈 불러오기

> 모듈을 사용하려면, import 명령어를 사용하여 모듈을 불러온다.. 모듈을 불러오면,해당 모듈 내의 함수나 클래스 등을 사용할 수 있다.

⚠️ 현재 디렉터리에 있는 파일이나 파이썬 라이브러리가 저장된 디렉터리에 있는 모듈만 불러올 수 있다.

```python
# fourCalModulTest.py
import FourCal

a = FourCal.FourCal(6, 3)
print(a.div()) # 2.0
```

<br>

> 별칭(alias) 지정하여 모듈을 불러올 수 있다.

```python
# fourCalModulTest.py
import FourCal as cal

a = cal.FourCal(6, 3)
print(a.div()) # 2.0
```

<br>

## 1.4 상속을 통한 기능 확장

> 모듈에 정의된 클래스를 상속받아 기능을 확장할 수 있다.

아래는 FourCal 클래스를 상속받아 0으로 나누는 오류를 처리하는 SafeFourCal 클래스이다.

```python
# fourCalModulTest.py
import FourCal

class SafeFourCal(FourCal.FourCal):
    def div(self):
        if self.second==0:
            return 0
        else:
            return self.first / self.second

a=SafeFourCal(6, 3) # 2.0
print(a.div())
```

<br>

## 1.5 모듈 이름 없이 함수 이름만 쓰기

> 클래스의 인스턴스 메서드가 아닌 `mod1.py` 정의

```python
# mod1.py
def add(a, b):
    return a + b

def sub(a, b):
    return a-b
```

- 첫 번쨰 방법: `,`로 구분

```python
# test1.py
from mod1 import add, sub
print(add(3, 4)) # 7
print(sub(3, 4)) # -1
```

<br>

- 두 번째 방법: `*` 문자 사용 (사용 주의)

```python
from mod1 import * # 모듈에 있는 모든 것
```

<br>

> ⚠️ 아래처럼 클래스의 경우 모듈 이름 없이 함수 이름만 사용하는 방식으로는 메서드를 호출할 수 없다.

이유는 클래스의 메서드는 클래스의 객체(인스턴스)를 통해 호출해야 하기 때문이다.

```python
class FourCal:
    def __init__(self, first, second):
        self.first = first
        self.second = second

    def add(self):
        return self.first + self.second

    def sub(self):
        return self.first - self.second

```

<br>

> 따라서, 아래와 같이 클래스를 사용하기 위해서는 먼저 클래스의 인스턴스를 생성한 후 해당 인스턴스를 통해 메서드를 호출해야 한다.

아래 코드는 add와 sub를 모듈에서 바로 가져와서 사용할 수 없다.

```python
from FourCal import add, sub

# add와 sub는 인스턴스 메서드이기 때문에,
# FourCal 클래스의 객체를 통해서만 호출할 수 있다.
print(add(3, 4))  # 오류
print(sub(3, 4))  # 오류
```

<br>

FourCal 클래스의 객체 a를 생성하고, 그 객체를 통해 add()와 sub() 메서드를 호출해야 정상적으로 동작한다.

```python
from FourCal import FourCal

# 인스턴스를 생성하므로 호출 가능.
a = FourCal(6, 3)
print(a.add())
print(a.div())
```

<br>

# 1.7 다른 위치의 모듈 불러오기

> 모듈이 하위 디렉터리에 있을 때는, 상대경로를 사용하여 해당 모듈을 불러올 수 있다.

car 디렉터리 내에 Car.py 모듈이 있고, 그 안에 정의된 Car 클래스를 main.py에서 불러와 사용해보자.

<br>

> 디렉토리 구조

```bash
project/
│
├── car/
│   └── Car.py    # Car 클래스 정의
│
└── main.py      # Car 클래스를 사용하는 메인 프로그램
```

<br>

> `Car.py` (Car 클래스를 정의한 파일):

```python
# Car.py
class Car:
        def __init__(self, make, model, year, color):
            self.make = make
            self.model = model
            self.year = year
            self.color = color

        def drive(self):
              print("This car is driving")

        def stop(self):
              print("This car is stopped")
```

<br>

> main.py에서 Car 클래스를 사용하기

하위 디렉터리 내에 있는 모듈을 사용할 때는 `from 디렉터리명.모듈명 import 클래스명` 형식을 사용하여 모듈을 불러올 수 있다.

```python
from car.Car import Car # car 디렉터리 내의 Car.py 모듈에서 Car 클래스를 임포트

car_1 = Car("Hyundai", "Grandeur", 2024, "black")
car_2 = Car("Kia", "Carnival", 2023, "white")

print(f"make={car_1.make}, model={car_1.model}, year={car_1.year}, color={car_1.color}")
car_1.drive()
```

![](/assets/images/2024/2024-11-27-19-35-13.png)

<br>

> 출력 결과

![](/assets/images/2024/2024-11-27-19-33-43.png)

<br>
