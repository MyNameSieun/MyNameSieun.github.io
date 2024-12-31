---
title: "[Python] #9 Error Handling (예외 처리)"
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

[[점프 투 파이썬↗️]](https://wikidocs.net/30)을 바탕으로 정리한 글입니다.
{: .notice--danger}

---

<br>

# 1. 예외 처리 개요

## 1.1 예외 처리란?

프로그램이 동작중에 발생하는 오류로 인해 프로그램이 정지 되지 않도록 방어적인 방법을 말한다.

<br>

## 1.2 오류는 언제 발생하는가?

```python
# 파일이 없을 때 발생하는 오류
f = open("나없는파일", 'r')  # FileNotFoundError

# 0으로 나눌 때 발생하는 오류
4 / 0  # ZeroDivisionError

# 리스트 인덱스 초과 시 발생하는 오류
a = [1, 2, 3]
a[3]  # IndexError

```

<br><br>

# 2. 오류 예외 처리 기법

> 오류 처리 구문은 조건문이랑 거의 구조가 비슷하다.

```python
try:
  # 오류가 발생할 수 있는 구문
except Exception as e:
  # 오류 발생하면 실행
else:
  # 오류 발생하지 않음
finally:
  # 무조건 마지막에 실행
```

아래 목차에서 하나씩 알아보도록 하자

<br>

## 2.1 try-except 문

```python
try:
    ...
except [발생오류 [as 오류변수]]:
    ...
```

```python
# try-except.py
try:
    a = [1, 2]
    print(a[3])
    4/0
except ZeroDivisionError as e:
    print("0으로 나눌 수 없습니다.")
    print(e)
except IndexError as e:
    print("인덱싱 할 수 없습니다.") # 인덱싱 할 수 없습니다.
    print(e) # list index out of range
```

<br>

## 2.2 try-except-else 문

```python
try:
  (...)
except:
  (...)
else:
  (...) # 에러의 발생 없는 경우, 실행

```

```python
# try-except-else.py
try:
    x = int(input("10을 나눌 숫자를 입력: "))
    y = 10 / x
except ZeroDivisionError: # 에러가 발생했을 때 실행
    print("0으로 나누는 것은 불가")
else: # 예외가 발생하지 않았을 때 실행
    print(y)
```

```python
# try_else.py
try:
    age=int(input('나이를 입력하세요: '))
except:
    print('입력이 정확하지 않습니다.')
else:
    if age <= 18:
        print('미성년자는 출입금지입니다.')
    else:
        print('환영합니다.')
```

<br>

## 2.3 try-except-finally 문

```python
# try-except-finally
try:
  (...)
except:
  (...)
finally:
  (...) # 중간에 오류가 발생하더라도 무조건 실행
```

```python
# try-except-finally.py
try:
    a = [1, 2]
    print(a[3])
    4/0
except Exception as e:
    print("에러 발생: ", e) # 에러 발생:  list index out of range
finally:
    print("정상 종료합니다.") # 정상 종료합니다.
```

<br>

## 2.4 try-except-else-finally 문

```python
try:
  (...)
except:
  (...)
else:
  (...)
finally:
  (...) # 무조건 마지막에 실행
```

```python
# try-except-else-finally.py
try:
    x = int(input("10을 나눌 숫자를 입력: "))
    y = 10 / x
except ZeroDivisionError: # 에러가 발생했을 때 실행
    print("0으로 나누는 것은 불가")
else: # 예외가 발생하지 않았을 때 실행
    print(y)
finally: # 예외 발생과 상관 없이 항상 실행
    print("코드 실행 종료")
```

<br><br>

# 3. 오류 회피하기

> 코드를 작성하다 보면 특정 오류가 발생할 경우 그냥 통과시켜야 할 때가 있을 때, 아래와 같이 `pass`를 사용하여 오류를 회피할 수 있다.

```python
# error_pass.py
try:
    f = open("나없는파일", 'r')
except FileNotFoundError:
    pass
```

<br><br>

# 5. 예외 만들기

> 예외도 클래스이다.사용자 정의 예외를 만들려면 파이썬 내장 클래스인 `Exception` 클래스를 상속하여 만들 수 있다.

```python
# error_make.py

# 사용자 정의 예외 클래스
class MyError(Exception):
    def __str__(self):
        return "허용되지 않는 별명입니다."

# 예외를 발생시키는 함수
def say_nick(nick):
    if nick == '바보':
        raise MyError()  # MyError 예외를 발생시킴
    print(nick)

# 예외 처리
try:
    say_nick("천사")
    say_nick("바보")  # 이 부분에서 예외가 발생
except MyError as e:
    print(e)  # "허용되지 않는 별명입니다." 출력
```

- MyError 클래스는 Exception을 상속받아 사용자가 정의한 예외 클래스이다.
- `__str__ `메서드는 print(e)처럼 오류 메시지를 print 문으로 출력할 경우에 호출되는 메서드이다.
- say_nick 함수는 '바보'라는 별명이 들어오면 MyError 예외를 발생시킨다.

<br><br>

# 6. 연습 문제

## 6.1 try-except 문

> 사용자에게 두 숫자를 입력받아 나누기를 수행하는 프로그램을 작성하세요.

첫 번째 숫자를 0으로 나누려고 할 때 ZeroDivisionError가 발생하면, 오류 메시지를 출력하세요.
그 외의 오류가 발생할 경우 "알 수 없는 오류가 발생했습니다."라고 출력하세요.

```python
try:
  num1 = int(input("첫 번쨰 숫자: "))
  num2 = int(input("두 번쨰 숫자: "))
  result = num2 / num1
  print(f"결과: {result}")
except ZeroDivisionError:
  print("0으로 나눌 수 없습니다.")
except Exception as e:
  print("알 수 없는 오류가 발생했습니다.")
```

<br>

> 사용자로부터 숫자 3개를 입력받아 리스트에 저장한 후, 이 리스트에서 인덱스 3의 값을 출력하는 프로그램을 작성하세요.

- (인덱스가 범위를 벗어난 경우 IndexError가 발생할 것입니다.)
- IndexError가 발생하면 "리스트 인덱스가 범위를 벗어났습니다."라는 메시지를 출력하세요.

```python
try:
  numbers=[]
  for i in range(3):
      numbers.append(int(input(f"{i+1}번째 숫자를 입력하세요: ")))
  print(numbers[3])
except IndexError:
  print("리스트가 인덱스 범위를 벗어났습니다.")
```

<br>

> 배열에서 요소 3을 출력하는 프로그램을 작성하세요. 요소 3이 없다면 "존재하지 않는 숫자입니다."를 출력하세요.

- 리스트 a는 [1, 2, 3]으로 고정되어 있습니다.
- 사용자는 0, 1, 2 중 하나의 값을 입력해야 합니다. 만약 이 외의 값을 입력하면 IndexError가 발생합니다.
- IndexError가 발생했을 경우, "존재하지 않는 숫자입니다."라는 메시지를 출력하도록 하세요.

```python
try:
  a=[1,2,3]
  num=int(input())
  value=a[num]
  print(f"입력한 인덱스 {num}에 해당하는 값은: {value}")
except IndexError:
  print("존재하지 않는 숫자입니다.")
```

<br>

## 6.2 try-except-else 문

> 사용자로부터 나이를 입력받는 프로그램을 작성하세요.

- 입력 값이 숫자가 아닌 경우 ValueError가 발생하므로 이를 처리하여 "잘못된 입력입니다."라는 메시지를 출력하세요.
- 나이가 18세 이하인 경우 "미성년자입니다."를 출력하고, 19세 이상인 경우 "성인입니다."를 출력하세요.

```python
try:
  age=int(input("나이를 입력하세요: "))
except ValueError:
  print("유효한 나이를 입력하세요.")
else:
    if(age>=18):
      print("성인")
    else:
      print("미성년자")
```

<br>

> 사용자에게 두 숫자를 입력받아 나누기를 수행하는 프로그램을 작성하세요.

첫 번째 숫자를 0으로 나누려고 할 때 ZeroDivisionError가 발생하면, 오류 메시지를 출력하세요.
그 외의 오류가 발생할 경우 "알 수 없는 오류가 발생했습니다."라고 출력하세요.

```python
try:
  num1 = int(input("첫 번쨰 숫자: "))
  num2 = int(input("두 번쨰 숫자: "))
  result = num2 / num1
except ZeroDivisionError:
  print("0으로 나눌 수 없습니다.")
except Exception as e:
  print("알 수 없는 오류가 발생했습니다.")
else:
  print(f"결과: {result}")
```

<br>

## 6.3 try-except-finally 문

> test.txt 파일을 열고 내용을 출력하려고 할 때, 파일이 없으면 오류 메시지를 출력하고, 파일을 닫는 코드가 항상 실행되도록 예외 처리를 해보세요.

```python
try:
    file = open("test.txt", "r")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("파일을 찾을 수 없습니다.")
finally:
    try:
        file.close()
    except NameError:
        pass  # 파일을 열지 못한 경우 예외 처리
```

<br>

> 사용자로부터 두 숫자를 입력받아 덧셈을 하고 결과를 출력하는 프로그램을 작성하세요.

- 입력이 잘못되면 예외를 처리하여 "잘못된 입력입니다."를 출력하고, 입력이 올바른 경우 덧셈 결과를 출력하세요.
- 이와 상관없이 마지막에 "프로그램 종료" 메시지가 출력되도록 하세요.

```python
try:
    num1 = int(input("첫 번째 숫자를 입력하세요: "))
    num2 = int(input("두 번째 숫자를 입력하세요: "))
    result = num1 + num2
except Exception:
    print("잘못된 입력입니다.")
else:
    print("덧셈 결과: ", result)
finally:
    print("프로그램 종료")
```

<br>

## 6.4 try-except-else-finally 문

> 사용자가 입력한 파일명을 열고, 파일이 존재하면 내용을 출력하며, 존재하지 않으면 오류 메시지를 출력하세요. 프로그램이 끝날 때 "작업 완료" 메시지가 출력되도록 예외 처리를 해보세요.

```python
try:
    filename = input("파일명을 입력하세요: ")
    file = open(filename, "r")
    content = file.read()
    print(content)
except FileNotFoundError:
    print("파일을 열 수 없습니다.")
else:
    print("파일 읽기 성공")
finally:
    print("작업 완료")
```

<br>

> 사용자로부터 숫자 두 개를 입력받아 나누기를 수행하는 프로그램을 작성하세요.

- 사용자로부터 숫자 두 개를 입력받아 나누기를 수행하는 프로그램을 작성하세요.
- 입력된 두 숫자가 모두 유효하다면 결과를 출력하고, 0으로 나누기를 시도하면 ZeroDivisionError가 발생하므로 이를 처리합니다.
- 에러가 발생하지 않으면 결과를 출력하고, 발생한 경우에는 오류 메시지를 출력하세요.
- 마지막에는 반드시 "코드 실행 종료"라는 메시지를 출력하세요.

```python
try:
    num1 = int(input("첫 번째 숫자를 입력하세요: "))
    num2 = int(input("두 번째 숫자를 입력하세요: "))
    result = num1 / num2
except ZeroDivisionError:
    print("0으로 나누는 것은 불가")
else:
    print("결과는: ", result)
finally:
    print("코드 실행 종료")
```

<br>
