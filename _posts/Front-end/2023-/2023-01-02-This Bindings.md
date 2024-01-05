---
title: "[JS] This Bindings"
categories: [JavaScript]
tag: [JavaScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

《 [실행 컨텍스트](https://velog.io/@sieunpark/JS-%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8) 》에 이어 작성하는 글입니다.

---

# ▶ this binding이란?

- 자바스크립트에서 this가 참조하는 것은 함수가 호출되는 방식에 따라 결정되는데, 이것을 "this binding"이라고 한다.

<br>

## ▷ 상황에 따라 달라지는 this

> 실행 컨텍스트 복습!

- 실행 컨텍스트는 실행할 코드에 제공할 환경 정보들을 모아놓은 객체이며, 그 객체 안에는 3가지가 존재한다고 하였다.
  ✓ VariableEnvironment
  ✓ LexicalEnvironment
  <span style="color:indianred"> ✓ ThisBindings</span>

<br>

### ① this의 결정

- this는 실행 컨텍스트가 생성될 때 결정된다.
- 이 말을 this를 bind한다(= 묶는다) 라고도 한다.
- 즉, this는 함수를 호출할 때 결정되는 것이다.

<br>

> 전역 공간에서의 this

- 전역 공간에서 this는 전역 객체를 가리킨다.
- 런타임 환경에 따라 this는 window(브라우저 환경) 또는 global(node 환경)를 각각 가리킨다.
- 런타임 환경?
  - 코드가 돌아가는 환경을 말한다.
  - 브라우저 환경과 node 환경이 있다.

<br>

- 브라우저 환경에서의 this 확인
  - window라는 객체가 나온다.
    ➡️ 브라우저에서의 this라는 전역 변수는 <span style="color:indianred">window 객체</span>를 의미한다는 것을 알 수 있다.
    ![](https://velog.velcdn.com/images/sieunpark/post/c98e4033-e7a0-4625-a50e-829f9f32fb23/image.png)![](https://velog.velcdn.com/images/sieunpark/post/2ec91eb6-ce63-44a4-a7c1-9bc51e566909/image.png)
  ```jsx
  console.log(this);
  console.log(window);
  console.log(this === window);
  ```

<br>

- node 환경에서의 this 확인

  - globle이라는 객체가 나온다.
    ➡️ node 환경에서의 this라는 전역 변수는 <span style="color:indianred">globle 객체</span>를 의미한다는 것을 알 수 있다.
    ![](https://velog.velcdn.com/images/sieunpark/post/dcddc208-922e-4aae-b50a-bf05689cf532/image.png)![](https://velog.velcdn.com/images/sieunpark/post/a87bd5f3-2aaf-43e0-8213-6302ffd70662/image.png)

  ```jsx
  console.log(this);
  console.log(global);
  console.log(this === global);
  ```

<br>

### ② 메서드로서 호출시, 메서드 내부에서의 this

> 함수 vs 메서드

- 함수와 메서드를 구분하는 기준은 <span style="color:indianred">독립성</span>이다.
  - 함수는 그 자체로 독립적인 기능을 수행한다. `함수명();`
  - 메서드는 자신을 호출한 대상 객체에 대한 동작을 수행한다. (종속적) `객체.메서드명();`

➡️ 즉, 함수는 호출의 주체를 명시할 수 없기 때문에 <span style="color:indianred">this가 전역 객체</span>이지만, 메서드는 <span style="color:indianred">this가 호출의 주체</span>가 된다!!

<br>

> this의 할당

- 함수
  - 호출 주체를 명시할 수 없기 때문에 <span style="color:indianred">this는 전역 객체</span>를 의미한다.

```jsx
var func = function (x) {
  console.log(this, x);
};
func(1); // Window { ... } 1
```

<br>

- 메서드
  - 호출 주체를 명시할 수 있기 때문에 <span style="color:indianred">this는 해당 객체</span>(obj)를 의미한다. (종속적)

```jsx
// obj는 곧 { method: f }를 의미한다.
var obj = {
  method: func,
};
obj.method(2); // { method: f } 2
```

<br>

> 함수로서의 호출과 메서드로서의 호출 구분 기준

- 호출의 주체가 있는지 여부에 따라 구분한다.
- 호출의 주체를 인지할 수 있는 방법 : 점(.) 또는 대괄호([])

```jsx
var obj = {
  method: function (x) {
    console.log(this, x);
  },
};
obj.method(1); // { method: f } 1
obj["method"](2); // { method: f } 2
```

<br>

### ③ 함수로서 호출시, 그 함수 내부에서의 this

> 함수 내부에서의 this

- 어떤 함수를 함수로서 호출할 경우, this는 지정되지 않는다. (호출 주체가 알 수 없기 때문)
- 실행 컨텍스트를 활성화할 당시 this가 지정되지 않은 경우, this는 전역 객체를 의미한다.
- 따라서, 함수로서 <span style="color:indianred">독립적으로⭐</span> 호출할 때는 <span style="color:indianred">this는 항상 전역객체</span>를 가리킨다.

<br>

> 메서드의 내부함수에서의 this

- 메서드의 내부라고 해도, 함수로서 호출한다면 this는 전역 객체를 의미한다.
- 아래 코드의 실행 결과 (1), (2), (3)을 예측해보자!

```jsx
var obj1 = {
  outer: function () {
    console.log(this); // (1)

    var innerFunc = function () {
      console.log(this); // (2), (3)
    };
    innerFunc();

    var obj2 = {
      innerMethod: innerFunc,
    };
    obj2.innerMethod();
  },
};
obj1.outer();
```

<br>

➡️ 실행 결과 : (1) : obj1, (2) : 전역객체, (3) : obj2
![](https://velog.velcdn.com/images/sieunpark/post/2d23eff4-c3a4-49ea-81c3-7782281cc77d/image.png)

<br>

> 메서드의 내부 함수에서의 this 우회

- 점(.) 또는 대괄호([]) 표기를 안하면 무조건 전역 객체를 바라보는 문제를 this를 우회하여 해결할 수 있다.
  <br>
- 변수 활용
  - 내부 스코프에 이미 존재하는 this를 별도의 변수(ex. self)에 할당하는 방법이다.

```jsx
var obj1 = {
  outer: function () {
    console.log(this); // (1) outer

    // AS-IS (this 유실)
    var innerFunc1 = function () {
      console.log(this); // (2) 전역객체
    };
    innerFunc1();

    // TO-BE (this 보존)
    var self = this; // this 저장
    var innerFunc2 = function () {
      console.log(self); // (3) outer
    };
    innerFunc2();
  },
};

// 메서드 호출 부분
obj1.outer();
```

<br>

- 《 [화살표 함수 ](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98)》 활용 (= this를 바인딩하지 않는 함수)
  - 실행 컨텍스트를 생성할 때 this 바인딩 과정 자체가 없기 때문에, this는 이전의 값-상위값-이 유지된다.
  - ES6에서는 함수 내부에서 this가 전역객체를 바라보는 문제 때문에 화살표 함수를 도입했다.
  - 일반 함수와 화살표 함수의 가장 큰 차이점은 <span style="color:indianred"> this binding 여부</span>이다!!

```jsx
var obj = {
  outer: function () {
    console.log(this); // (1) obj
    var innerFunc = () => {
      console.log(this); // (2) obj
    };
    innerFunc();
  },
};

obj.outer();
```

<br>

> 콜백 함수 호출 시 그 함수 내부에서의 this

- 아래 코드는 모두 《 [콜백 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》이다.
- 즉, 콜백 함수도 함수이므로 무조건 전역 객체를 바라보게 된다 ➡️ this를 잃어버리게 되는 문제 발생
- 단, 콜백 함수에 별도로 this를 지정한 경우는 예외로 한다.

```jsx
// 별도 지정 없음 : 전역객체
setTimeout(function () {
  console.log(this);
}, 300);

// 별도 지정 없음 : 전역객체
[1, 2, 3, 4, 5].forEach(function (x) {
  console.log(this, x);
});

// addListener 안에서의 this는 항상 호출한 주체의 element를 return하도록 설계되었음
// 따라서 this는 전역 객체가 아닌 button을 의미함
document.body.innerHTML += '<button id="a">클릭</button>';
document.body.querySelector("#a").addEventListener("click", function (e) {
  console.log(this, e);
});
```

- setTimeout 함수, forEach 메서드는 콜백 함수를 호출할 때 대상이 될 this를 지정하지 않으므로, this는 곧 window객체
- addEventListner 메서드는 콜백 함수 호출 시, 자신의 this를 상속하므로, this는 addEventListner의 앞부분(button 태그)

<br>

> 《 [생성자 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》 내부에서의 this

- 생성자 함수란 객체를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스라고 한다.
- 생성자 함수를 사용하면 비슷한 객체를 여러개 만들 수 있고 상속을 구현할 수 있다.

```jsx
var Cat = function (name, age) {
  this.bark = "야옹";
  this.name = name;
  this.age = age;
};

var choco = new Cat("초코", 7); //this : choco
var nabi = new Cat("나비", 5); //this : nabi
```

<br>

## ▷ 명시적 this binding

- 자동으로 부여되는 상황별 this의 규칙을 깨고 this에 별도의 값을 저장하는 방법을 말한다.
- 크게, call, apply, bind 메서드가 있다.

### ① call 메서드

> 호출 주체인 함수를 즉시 실행하는 명령어이다.

- call명령어를 사용하여, 첫 번째 매개변수에 this로 binding할 객체를 넣어주면 명시적으로 binding할 수 있다.

- 전역 객체를 바라보는 현상에서의 명시적 바인딩

```jsx
var func = function (a, b, c) {
	console.log(this, a, b, c);
};

// no binding
func(1, 2, 3); // Window{ ... } 1 2 3

// 명시적 binding
// func 안에 this에는 {x: 1}이 binding 된다.
func.call({ x: 1 }, 4, 5, 6}; // { x: 1 } 4 5 6
```

<br>

- 호출 주체가 있는 경우에서의 명시적 바인딩

```jsx
var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  },
};

// method 함수 안의 this는 항상 obj를 가리킨다.
obj.method(2, 3); // 1 2 3

// 명시적 binding
obj.method.call({ a: 4 }, 5, 6); // 4 5 6
```

<br>

### ② apply 메서드

> call 메서드와 완전히 동일하지만, this에 binding할 객체는 똑같이 넣어주고 나머지 부분만 배열 형태로 넘겨준다.

```jsx
var func = function (a, b, c) {
  console.log(this, a, b, c);
};
func.apply({ x: 1 }, [4, 5, 6]); // { x: 1 } 4 5 6

var obj = {
  a: 1,
  method: function (x, y) {
    console.log(this.a, x, y);
  },
};

obj.method.apply({ a: 4 }, [5, 6]); // 4 5 6
```

<br>

### ③ call / apply 메서드 활용

> 유사 배열 객체(array-like-object)에 배열 메서드를 적용

중요한 개념이므로 따로 포스팅하였다.
자세한 내용은《 [유사 배열 객체](https://velog.io/@sieunpark/JS-%EC%9C%A0%EC%82%AC-%EB%B0%B0%EC%97%B4-%EA%B0%9D%EC%B2%B4) 》을 참고하자.

<br>

> Array.from 메서드(ES6)

- call, apply를 통해 this binding을 하는 것뿐만이 아니라 객체 → 배열로의 형 변환 만을 위해서도 쓸 수 있다.
- 하지만, 원래 의도와는 거리가 멀기 때문에 ES6에서는 Array.from 이 등장하게 되었다.

```jsx
// 유사배열
var obj = {
  0: "a",
  1: "b",
  2: "c",
  length: 3,
};

// 객체 -> 배열
var arr = Array.from(obj);

// 찍어보면 배열이 출력된다.
console.log(arr); // [ 'a', 'b', 'c' ]
```

<br>

> 생성자 내부에서 다른 생성자를 호출 (공통된 내용의 반복 제거)

- Student, Employee 모두 Person이기 때문에, name과 gender 속성 모두 필요하다.
- 따라서 Student와 Employee 인스턴스를 만들 때 마다 세 가지 속성을 모두 각 《 [생성자 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》에 넣기 보다는 Person이라는 생성자 함수를 별도로 빼는게 ‘구조화’에 도움이 될 것이다.

<br>

구조화 전

```jsx
function Student(name, gender, school) {
  this.name = name;
  this.gender = gender;
  this.school = school;
}
function Employee(name, gender, company) {
  this.name = name;
  this.gender = gender;
  this.company = company;
}
var kd = new Student("길동", "male", "서울대");
var ks = new Employee("길순", "female", "삼성");
```

<br>

구조화 후 (생성자 함수를 별도로 빼냄)

```jsx
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}
function Student(name, gender, school) {
  Person.call(this, name, gender); // 여기서 this는 student 인스턴스
  this.school = school;
}
function Employee(name, gender, company) {
  Person.apply(this, [name, gender]); // 여기서 this는 employee 인스턴스
  this.company = company;
}
var kd = new Student("길동", "male", "서울대");
var ks = new Employee("길순", "female", "삼성");
```

<br>

> 여러 인수를 묶어 하나의 배열로 전달할 때 apply 사용할 수 있다.

apply를 통해 비효율적인 예시를 효율적인 예시로 바꿔보자
➡️ `(apply ->({},[])` 첫 번째 인자로 this로 엮을 객체, 두 번째 인자로 배열을 받는 것을 활용)

```jsx
//비효율
var numbers = [10, 20, 3, 16, 45];
var max = (min = numbers[0]);
numbers.forEach(function (number) {
  // 현재 돌아가는 숫자가 max값 보다 큰 경우
  if (number > max) {
    // max 값을 교체
    max = number;
  }

  // 현재 돌아가는 숫자가 min값 보다 작은 경우
  if (number < min) {
    // min 값을 교체
    min = number;
  }
});

console.log(max, min);
```

<br>

apply를 적용하여 코드를 짧고 가독성 있게 수정해보자

```jsx
//효율
var numbers = [10, 20, 3, 16, 45];
var max = Math.max.apply(null, numbers);
var min = Math.min.apply(null, numbers);
console.log(max, min);
```

<br>

《 [전개 구문(Spread Operation)](https://velog.io/@sieunpark/JS-%EB%82%98%EB%A8%B8%EC%A7%80-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EC%99%80-%EC%A0%84%EA%B0%9C-%EA%B5%AC%EB%AC%B8) 》을 사용하면 더 간편하게 해결도 가능하다. (제일 많이 쓰임)

```jsx

const numbers = [10, 20, 3, 16, 45];
const max = Math.max(...numbers);
const min = Math.min(...numbers);
console.log(max min);
```

<br>

### ④ bind 메서드

> - this를 바인딩 하는 메서드

- call, apply와 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환한다.

- 목적
  - 함수에 this를 미리 적용한다. (즉시 실행하는 것이 아니므로)
  - 부분 적용 함수 구현할 때 용이하다.⭐

```jsx
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};
func(1, 2, 3, 4); // window객체

// 함수에 this 미리 적용
var bindFunc1 = func.bind({ x: 1 }); // 바로 호출되지 않는다.
bindFunc1(5, 6, 7, 8); // { x: 1 } 5 6 7 8

// 부분 적용 함수 구현
var bindFunc2 = func.bind({ x: 1 }, 4, 5); // 4와 5를 미리 적용
bindFunc2(6, 7); // { x: 1 } 4 5 6 7
bindFunc2(8, 9); // { x: 1 } 4 5 8 9
```

<br>

> name 프로퍼티

bind 메서드를 적용해서 새로 만든 함수는 name 프로퍼티에 ‘bound’ 라는 접두어가 붙는다. (추적 용이)

```jsx
var func = function (a, b, c, d) {
  console.log(this, a, b, c, d);
};
var bindFunc = func.bind({ x: 1 }, 4, 5);

// func와 bindFunc의 name 프로퍼티의 차이를 살펴보자
console.log(func.name); // func
console.log(bindFunc.name); // bound func
```

<br>

> 상위 컨텍스트의 this를 내부함수나 콜백 함수에 전달하기

- 내부함수
  - 메서드의 내부함수에서 메서드의 this를 그대로 사용하기 위한 방법이다.
  - 이전에는 내부함수에 this를 전달하기 위해 self를 썼었지만 지금은 사용하지 않는다.
    ![](https://velog.velcdn.com/images/sieunpark/post/1c031f98-1298-48ec-a39b-bc9f1c2331dc/image.png)
  - self 등의 변수를 활용한 우회법보다 call, apply, bind를 사용하면 깔끔하게 처리가 가능하기 때문이다.

```jsx
var obj = {
  outer: function () {
    console.log(this); // obj
    var innerFunc = function () {
      console.log(this);
    };

    // call을 이용해서 즉시실행하면서 this를 넘겨주었다.
    innerFunc.call(this); // obj
  },
};
obj.outer();
```

<br>

- 이번엔, call이 아니라 bind를 이용해보자.

```jsx
var obj = {
  outer: function () {
    console.log(this);
    var innerFunc = function () {
      console.log(this);
    }.bind(this); // innerFunc에 this를 결합한 새로운 함수를 할당
    innerFunc();
  },
};
obj.outer();
```

<br>

> 《 [콜백 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》

- 콜백함수도 함수이기 때문에, 함수가 인자로 전달될 때는 함수 자체로 전달된다. (this가 유실)
- bind메서드를 이용해 this를 입맛에 맞게 변경이 가능하다.

```jsx
var obj = {
  logThis: function () {
    console.log(this);
  },
  logThisLater1: function () {
    // 0.5초를 기다렸다가 출력 (정상동작 X)
    // 콜백함수도 함수이기 때문에 this를 bind해주지 않아서 잃어버림 (유실)
    setTimeout(this.logThis, 500);
  },
  logThisLater2: function () {
    // 1초를 기다렸다가 출력 (정상동작)
    // 콜백함수에 this를 bind 해주었기 때문
    setTimeout(this.logThis.bind(this), 1000);
  },
};

obj.logThisLater1();
obj.logThisLater2();
```

<br>

> 《 [화살표 함수](https://velog.io/@sieunpark/JS-%ED%95%A8%EC%88%98) 》의 예외사항

- 이전 포스팅에서 강조 했듯 실행 컨텍스트 생성 시, "<span style="color:indianred"> 화살표 함수는 this를 바인딩 하는 과정이 없다."</span>고 하였다!
- 즉, 화살표 함수 내부에는 this의 할당과정(바인딩 과정)이 아예 없으며, 스코프체인상 가장 가까운 this에 접근하게 된다.
- this 우회, call, apply, bind보다 편리한 방법이다.

```jsx
var obj = {
	outer: function () {
		console.log(this);
		var innerFunc = () => {
			console.log(this);
		};
		innerFunc();
	};
};
obj.outer();
```

<br>

---

<br>

# ▶ 문제📒

> - 출력의 결과를 제출해주세요, 그리고 그 이유를 최대한 상세히 설명해주세요

- **주의사항 : 브라우저에서 테스트해주세요(Chrome → 개발자 도구 → 콘솔에 입력하여 실행)**

```jsx
var fullname = "Ciryl Gane";

var fighter = {
  fullname: "John Jones",
  opponent: {
    fullname: "Francis Ngannou",
    getFullname: function () {
      return this.fullname;
    },
  },

  getName: function () {
    return this.fullname;
  },

  getFirstName: () => {
    return this.fullname.split(" ")[0];
  },

  getLastName: (function () {
    return this.fullname.split(" ")[1];
  })(),
};

console.log("Not", fighter.opponent.getFullname(), "VS", fighter.getName());
console.log(
  "It is",
  fighter.getName(),
  "VS",
  fighter.getFirstName(),
  fighter.getLastName
);
```

<br>

## ▷ 풀이

위 코드를 브라우저 상에서 출력하면 아래와 같은 결과를 얻을 수 있다.

```jsx
Not Francis Ngannou VS John Jones
VM62:27 It is John Jones VS Ciryl Gane
```

<br>

이 결과는 JavaScript의 this 바인딩 방식과 화살표 함수의 특성 때문이다.

- `fighter.opponent.getFullname`
  - 이 함수는 fighter.opponent 객체의 메서드로 호출되므로, this는 fighter.opponent를 가리킨다.
  - 따라서 this.fullname은 "Francis Ngannou"를 반환한다.
    <br>
- `fighter.getName`
  - 이 함수는 fighter 객체의 메서드로 호출되므로, this는 fighter를 가리킨다.
  - 따라서 this.fullname은 "John Jones"를 반환한다.
    <br>
- `fighter.getFirstName`⭐
  - 이 함수는 화살표 함수로 정의되었다.
  - 화살표 함수는 this를 바인딩하지 않고, 스코프체인상 가장 가까운 this에 접근하게 된다.
  - 이 경우, 화살표 함수는 전역 스코프에서 정의되었으므로, this는 전역 객체를 가리킨다.
    <br>
- `fighter.getLastName`
  - 이 함수는 즉시 실행 함수로 정의되었고, 전역 스코프에서 실행되었다.
  - 따라서 this는 전역 객체를 가리킨다.
  - 호출 부가 없다는 것이 주목하자

<br>

## ▷ 전체 코드

```jsx
var fullname = "Ciryl Gane";

var fighter = {
  fullname: "John Jones",
  opponent: {
    fullname: "Francis Ngannou",
    getFullname: function () {
      // 1. 객체 this 바인딩 : 프란시스 은가누
      return this.fullname;
    },
  },

  getName: function () {
    // 2. 객체 this 바인딩 : 존 존스
    return this.fullname;
  },

  getFirstName: () => {
    // 3. 함수 this 바인딩 : 시릴
    return this.fullname.split(" ")[0];
  },

  getLastName: (function () {
    // 3. 함수 this 바인딩 : 시릴
    return this.fullname.split(" ")[1];
  })(),
};

console.log("Not", fighter.opponent.getFullname(), "VS", fighter.getName());
console.log(
  "It is",
  fighter.getName(),
  "VS",
  fighter.getFirstName(),
  fighter.getLastName
);
```

<br>

---

<br>

# 📎참조

- https://seungtaek-overflow.tistory.com/21
