---
title: "[TS] #3 TSC 및 tsconfig"
categories: [TypeScript]
toc_label: Contents
toc: true
toc_sticky: true
author_profile: true
sidebar:
  nav: "counts"
---

<br>

# 1. 컴파일러와 TSC

## 1.1 컴파일러의 개념

- 컴파일러란 프로그래밍 언어로 작성된 소스 코드 → 다른 프로그래밍 언어로 변환하는 도구이다.
- 변환 과정에서 컴파일러는 소스 코드의 구문과 구조를 검사하여 문제가 없는지 확인한다.

<br>

## 1.2 컴파일러의 역할

> 1️⃣ 타입 검사를 해준다.

<span style="color:CornflowerBlue">TS 컴파일러인 TSC</span>는 소스 코드의 정적 타입 검사를 수행하여 개발자가 코드에서 타입 관련 오류를 미리 발견하고 수정할 수 있게 한다.

<br>

> 2️⃣ 코드 변환을 해준다.

C언어 컴파일러가 C언어에서 기계어로 코드 변환을 해주는 것처럼, 타입스크립트 컴파일러인 TSC는 TypeScript → JavaScript 코드 변환을 해준다.

<br>

> 3️⃣ 자동으로 최적화를 해준다.

코드가 최적화되면 전반적인 어플리케이션 실행 시간이 더 빨라진다.

<br>

## 1.3 동적 언어 JS

> JavaScript는 동적 언어(=인터프리터 언어)이기 때문에 기계어로 변환될 필요가 없다.

- Node.js나 Chrome → JavaScript를 실행할 때는 V8 엔진이 코드 해석 및 실행을 한다.
- Firefox → JavaScript를 실행할 때는 SpiderMonkey가 코드 해석 및 실행을 한다.

<br>

➡️ 정리하면, 정적 언어(=컴파일 언어)는 기계어로 변환이 되어야 하지만, 동적 언어(=인터프리터 언어)는 엔진이 코드를 한 줄씩 실행하면서 동적으로 해석한다.

<br>

## 1.4 TSC 명령어

자세한 명령어 옵션 확인하기 : [tsc CLI Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

<br>

- `tsc —-init`
  - tsconfig.json이 생성되는 명령어이다.
- `tsc index.ts`
  - index.ts를 컴파일 한다.
  - **.ts**는 **TypeScript 파일의 확장자**이다.
- `tsc src/*.ts`
  - src 디렉토리 안에 있는 모든 TypeScript 파일을 컴파일한다.
- `tsc index.js --declaration --emitDeclarationOnly`
  - **@types** 패키지를 위한 .d.ts 파일 생성을 하는 명령이다.
  - TypeScript로 작성된 모듈이 아니라 JavaScript로 작성된 모듈에 타입 선언을 제공할 때 유용하게 쓰인다.

<br><br>

# 2. tsconfig.json 개요

## 2.1 tsconfig.json 개념

> tsconfig.json은 TS 프로젝트의 설정 파일이다.

- TypeScript 컴파일러(tsc)가 이 파일을 참고하여 어떤 파일을 컴파일할지, 그리고 어떤 규칙과 설정을 따를지를 결정한다
- 이 파일을 설정함으로써 프로젝트 전반에 걸쳐 일관된 컴파일 규칙을 적용할 수 있다.

<br>

## 2.2 tsconfig.json 예시

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "strict": true,
    "outDir": "dist",
    "sourceMap": true,
    "lib": ["ES6"]
  }
}
```

<br>

## 2.2 tsconfig.json 주요 옵션

옵션 매뉴얼 : [https://www.typescriptlang.org/ko/tsconfig](https://www.typescriptlang.org/ko/tsconfig)

> 주요 옵션에 살펴보기에 앞서, 아래 설정을 마치도록 하자.

- strict 옵션은 true로 설정하기
- sourceMap 옵션은 개발 환경에서 true로 설정하기

<br>

### 2.2.1 compilerOptions (컴파일러 옵션)

| 옵션            | 설명                                                                                                                                                                                                                                                                                                                                                                  |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| target          | 어떤 버전의 JavaScript로 컴파일할지 지정한다. 예: ES5, ES6, ESNext                                                                                                                                                                                                                                                                                                    |
| module          | - 어떤 모듈 시스템을 사용할지 설정한다.<br> - target 옵션과는 서로 독립적인 관계니 프로젝트의 요구사항에 따라 옵션을 설정하면 된다. 예: CommonJS, ESNext, AMD                                                                                                                                                                                                         |
| strict          | - 엄격한 타입 검사를 활성화한다. (TypeScript의 장점을 살리기 위해 권장)<br> - TypeScript 컴파일러가 보다 엄격한 타입 검사를 수행해 코드의 실수를 미리 찾아낼 수 있다.                                                                                                                                                                                                 |
| esModuleInterop | CommonJS와 ES 모듈 간의 상호 운용성을 활성화한다.                                                                                                                                                                                                                                                                                                                     |
| outDir          | - 컴파일된 파일이 출력될 디렉터리를 지정한다.<br> - 예를 들어, `"outDir": "dist"`로 설정하면 컴파일된 파일들이 `dist` 폴더에 저장된다.                                                                                                                                                                                                                                |
| rootDir         | 소스 파일의 기본 디렉터리를 지정한다.                                                                                                                                                                                                                                                                                                                                 |
| sourceMap       | - 컴파일된 JavaScript 파일에 소스 맵을 생성할지 여부를 설정한다.<br> - 소스 맵을 사용하면 실행 중에 에러가 발생했을 때 원래 TypeScript 소스 코드의 위치를 확인할 수 있다.<br> - 코드 디버깅에 매우 큰 도움이 되기 때문에 **개발 환경**에서는 true로 설정하는 게 좋다.<br> - 프로덕션 환경에서는 용량이나 성능상의 이유로 sourceMap을 사용하지 않는 것이 나을 수 있다. |
| jsx             | JSX를 사용할 때 어떤 방식으로 컴파일할지 지정한다. 예: react, react-jsx                                                                                                                                                                                                                                                                                               |
| noImplicitAny   | 타입을 명시하지 않으면 에러를 발생시킨다. (암시적 any 타입 사용을 금지)                                                                                                                                                                                                                                                                                               |
| skipLibCheck    | 외부 라이브러리의 타입 검사를 건너뛴다.                                                                                                                                                                                                                                                                                                                               |
| lib             | - TypeScript 컴파일러가 참조할 표준 라이브러리의 목록을 지정한다.<br> - 예를 들어, `"lib": ["ES6"]`으로 설정하면 ES6 라이브러리를 참조한다. <br> - 이 옵션은 프로젝트에서 사용하는 표준 라이브러리의 기능을 명시적으로 지정할 수 있다.                                                                                                                                |

<br>

### 2.2.2 include , exclude 옵션

> include

tsc가 컴파일을 할 때 포함하거나 제외할 파일이나 디렉터리를 지정하는 옵션이다.

```json
- “include": ["src/**/*"]
```

<br>

> exclude

컴파일에서 제외할 파일이나 디렉터리를 지정하는 옵션이다.

```json
// node_modules, dist 디렉토리 밑의 친구들은 컴파일 대상에서 제외하겠다는 의미
"exclude": ["node_modules", "dist"]
```

<br>

### 2.2.3 files

- 명시적으로 컴파일할 파일 목록을 지정할 수 있다.
- `include`나 `exclude` 대신 사용할 수 있다.

<br>

### 2.2.4 extends

- 다른 tsconfig.json 파일을 확장하여 설정을 상속받을 수 있다.
- 이를 통해 공통 설정을 여러 프로젝트에서 재사용할 수 있다.

```json
"extends": "./base-tsconfig.json"
```

<br><br>

# 3. tsconfig를 활용하여 컴파일 하기

- TypeScript 파일(.ts)은 브라우저가 직접 읽을 수 없으므로, 이를 JavaScript(.js)로 변환하는 과정이 필요하다.
- `tsc` 명령어를 실행하면 tsconfig.json 파일의 설정에 따라 TypeScript 파일이 컴파일된다.

![](/assets/images/2024/2024-09-18-15-56-52.png)

<br>

## 3.1 한 번만 컴파일

> TypeScript 파일을 한 번만 컴파일하려면 `tsc` 명령어를 사용한다.

```shell
tsc
```

<br>

## 3.2 실시간 컴파일

> 파일이 변경될 때마다 자동으로 컴파일하도록 하려면 `-w(watch)` 플래그를 사용한다.

-w 플래그를 사용하면 파일의 변경을 감지하고, 변경이 있을 때마다 자동으로 TypeScript 파일을 JavaScript로 변환한다.

```shell
tsc -w
```

<br>

## 3.3 특정 파일만 컴파일

> 특정 파일만 컴파일하고 싶다면, `tsc` 명령어에 파일 경로를 직접 지정할 수 있다.

```shell
tsc 파일명.ts
```

<br><br>

# 4. .d.ts 파일(참고)

## 4.1 .d.ts 파일 개념

> `d.ts` 파일을 통해서 js 파일의 타입으로 인식을 할 수 있다.

- TypeScript 타입 정의 파일이다.
- JavaScript 라이브러리에 대한 타입 정보를 제공하며 타입 추론도 가능하다.
- JavaScript 라이브러리를 TypeScript에서 쓰려면 해당 라이브러리에 대한 `.d.ts` 파일만 제공을 해주면 된다. (한 줄도 수정하지 않고 그대로 사용 가능)

<br>

## 4.2 JS 라이브러리를 TS 프로젝트에서 사용하기

① 아래의 명령어를 터미널에서 실행하자

```
npm init -y
```

![](/assets/images/2024/2024-03-06-13-57-54.png)

<br>

② 다음으로 tsconfig.json을 생성하여 TypeScript 프로젝트로 변환하면 된다.

```js
tsc --init
```

![](/assets/images/2024/2024-03-06-13-58-52.png)

<br>

③ tsconfig.json을 열어서 아래의 옵션을 주석 해제하여 true로 설정하자

```json
"allowJs": true // TypeScript 프로젝트에 JavaScript 파일 허용 여부
"checkJs": true // JavaScript 파일 타입 체크 여부
```

<br>

④ 그 후, TypeScript에서 사용하고 싶은 커스텀 JavaScript 라이브러리(test.js)를 만들면 된다.

```js
/**
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
export function add(a, b) {
  return a + b;
}
```

주석으로 매개변수 a와 b의 타입이 숫자이며, 함수의 반환 값의 타입이 숫자임을 명시하고 있다.

1. 위의 주석문은 **JSDoc**이라고 한다.
2. JSDoc은 API의 시그니처 (인자, 리턴 타입)를 설명하는 HTML 문서 생성기이다.
3. JSDoc으로 자바스크립트 소스코드에 타입 힌트를 제공할 수 있다.

<br><br>

⑤ 이제, JSDoc으로 타입 힌트가 제공된 test.js의 .d.ts 파일을 만들자.

아래의 명령어를 복사 + 붙여넣기를 해서 터미널에서 실행하자.

```js
npx tsc test.js --declaration --allowJs --emitDeclarationOnly --outDir types
```

![](/assets/images/2024/2024-03-07-12-43-16.png)

<br>

⑥ types/test.d.ts 파일을 확인하면 다음과 같이 생성이 되어있다.

```js
/**
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
export function add(a: number, b: number): number;
```

<br>

⑦ test.js 파일을 참조할 foo.ts 파일을 새로 만들어보자

```js
import { add } from "./test";
console.log(add(1, 2));
```

![](/assets/images/2024/2024-03-07-14-24-49.png)

<br>

⑧ 이제 foo.ts 파일을 실행시켜 보자

실행하면 3이라는 숫자가 뜨며 정상적으로 실행이 됨을 확인 할 수 있다.

```js
npx ts-node foo.ts
```

![](/assets/images/2024/2024-03-07-14-25-10.png)

<br><br>
