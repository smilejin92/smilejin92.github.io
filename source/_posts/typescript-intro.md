---
title: TypeScript Intro
thumbnail: /thumbnails/typescript.jpg
tag: TypeScript
categories:
- [TIL, TypeScript]
---

* 타입이 있는 자바스크립트
* 변수에는 타입이 없다 (`var`, `let`, `const`).

* 값에는 타입이 있다 (var x = `1` ). 
* 값에 의해서 변수의 타입이 추론된다 (타입 추론)
* 자바스크립트는 동적 타입 언어이다 (타입이 동적으로 변할 수 있음).

왜 타입을 만들었나?

- 값을 메모리에 집어 넣을때 몇 바이트를 집어 넣는지 정해놓기 위해
- 메모리를 절약하기 위해
- 값을 가져올 때도 확보한 메모리를 통째로 들고와야함



자바스크립트는 모든 변수가 8바이트

* 개발자의 편리를 위해서



변수: 재할당이 여러번 됨 vs. 상수: 재할당이 한번 (undefined -> something)



아스키코드는 모두 1바이트로 표현할 수 있음

유니코드는 2바이트로 표현

이모지는 2~4바이트



자바스크립트 - 인터프리터 (트랜스파일링 없이 바로 실행) 런타임에러

타입스크립트 - 자바스크립트로 트랜스파일하는 과정에서 컴파일 에러. 런타임 에러전에 디버깅 가능, 대규모 프로젝트에 적합


---

### Introduction

자바스크립트는 C나 Java와 같은 C-family 언어와는 구별되는 특징을 가지고 있다.

* Prototype 기반 객체 지향 언어
* 함수 레벨 스코프와 `this`
* 동적 타입 (dynamic typed) 언어 혹은 느슨한 타입 (loosely typed) 언어

이와 같은 특징은 클래스 기반 객체지향 언어(Java, C++, C#)에 익숙한 개발자를 혼란스럽게 하며, 코드의 복잡성을 높이고, 디버깅과 테스트 공수를 증가시키는 등의 문제를 일으킬 수 있다. 따라서 규모가 큰 프로젝트에서는 자바스크립트를 사용함에 있어 주의하여야 한다.

이같은 자바스크립트의 태생적 문제를 극복하고자 CoffeeScript, Dart, Haxe와 같은 AltJS (자바스크립트의 대체 언어)가 등장했다.

TypeScript 또한 **자바스크립트 대체 언어의 하나로써 자바스크립트(ES5)의 Superset(상위확장)이다**. C#의 창시자인 Anders Hejlsberg(아네르스 하일스베르)가 개발을 주도한 TypeScript는 Microsoft에서 2012년 발표한 오픈소스로, 정적 타이핑을 지원하며 ES6(ECMAScript 2015)의 클래스, 모듈 등과 ES7 Decorator 등을 지원한다.

![](/images/typescript/typescript-superset.png)

TypeScript는 ES5의 Superset이므로 기존의 자바스크립트(ES5) 문법을 그대로 사용할 수 있다. 또한, ES6의 새로운 기능들을 사용하기 위해 Babel과 같은 별도의 트랜스파일러를 사용하지 않아도 ES6의 새로운 기능을 기존의 자바스크립트 엔진(브라우저 또는 Node.js)에서 실행할 수 있다.



### TypeScript의 장점

**1. 정적 타입**

TypeScript를 사용하는 가장 큰 이유 중 하나는 정적 타입을 지원한다는 것이다. 아래 함수 예제를 살펴보자.

```javascript
function sum(a, b) {
  return a + b;
}
```

위 함수의 의도는 아마 2개의 숫자 타입 인수를 전달 받아 그 합을 반환하려는 것으로 추측된다. 하지만 코드상으로는 어떤 타입의 인수를 전달하여야 하는지, 어떤 타입의 반환값을 리턴해야 하는지 명확하지 않다. 따라서 위 함수는 아래와 같이 호출될 수 있다.

```javascript
function sum(a, b) {
  return a + b;
}

sum('x', 'y'); // 'xy'
```

위 코드는 자바스크립트 문법상 어떠한 문제도 없으므로 자바스크립트 엔진은 아무런 이의 제기 없이 코드를 실행한다. 이러한 상황이 발생한 이유는 변수나 반환값의 타입을 사전에 지정하지 않는 자바스크립트의 동적 타이핑에 의한 것이다.

위 함수를 TypeScript의 정적 타입을 사용하여 다시 작성 해보자.

```javascript
function sum(a: number, b: number) {
  return a + b;
}

sum('x', 'y');
// error TS2345: Argument of type '"x"' is not assignable to parameter of type 'number'.
```

TypeScript는 정적 타입을 지원하므로 **컴파일 단계에서 오류를 포착할 수 있는 장점이 있다.** 명시적인 정적 타입 지정은 개발자의 의도를 명확하게 코드로 기술할 수 있다. 이는 코드의 가독성을 높이고 코드를 예측할 수 있게 하여 디버깅을 쉽게 한다.

**2. 도구의 지원**
IDE(통합개발환경)를 포함한 다양한 도구의 지원을 받을 수 있다. IDE와 같은 도구에 타입 정보를 제공함으로써 높은 수준의 인텔리센스, 코드 어시스트, 타입 체크, 리팩토링 등을 지원받을 수 있으며 이러한 도구의 지원은 대규모 프로젝트를 위한 필수 요소이기도 하다.

**3. 강력한 객체지향 프로그래밍 지원**
인터페이스, 제네릭 등과 같은 강력한 객체지향 프로그래밍 지원은 크고 복잡한 프로젝트의 코드 기반을 쉽게 구성할 수 있도록 도우며, Java, C# 등의 클래스 기반 객체지향 언어에 익숙한 개발자가 자바스크립트 프로젝트를 수행하는 데 진입 장벽을 낮추는 효과도 있다.

**4. ES6 / ES Next 지원**
TypeScript는 아직 ECMAScript 표준에 포함되지는 않았지만 표준화가 유력한 스펙을 선제적으로 도입하므로 새로운 스펙의 유용한 기능을 안전하게 도입하기에 유리하다.

### 개발환경 구축
TypeScript 파일(.ts)은 브라우저에서 동작하지 않으므로 TypeScript 컴파일러를 이용해 자바스크립트 파일로 변환해야 한다. 이를 컴파일 또는 트랜스파일링이라 한다.

TypeScript 컴파일러를 설치하여 TypeScript 개발환경을 구축하고 TypeScript 컴파일러의 사용 방법에 대해 살펴보자.

**1. [Node.js](https://nodejs.org/en/) 설치**

**2. TypeScript 컴파일러 설치 및 사용법**
Node.js를 설치하면 npm도 같이 설치된다. 다음과 같이 터미널에서 npm을 사용하여 TypeScript를 전역에 설치한다.

```bash
$ npm install -g typescript
```

설치가 완료되면 버전을 출력하여 TypeScript의 설치를 확인한다.

```bash
$ tsc -v
```

TypeScript 컴파일러(tsc)는 TypeScript 파일(.ts)을 자바스크립트 파일로 트랜스파일링한다.

> 컴파일은 일반적으로 소스 코드를 바이트 코드로 변환하는 작업을 의미한다. TypeScript 컴파일러는 TypeScript 파일을 자바스크립트 파일로 변환하므로 컴파일보다는 트랜스파일링(Transpiling)이 보다 적절한 표현이다.

트랜스파일링을 실행해보기 위해 아래와 같은 파일을 작성해보자. 참고로 TypeScript 파일의 확장자는 .ts이다.

```javascript
// person.ts
class Person {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  sayHello(): string {
    return "Hello, " + this.name;
  }
}

const person = new Person('Kim');
console.log(person.sayHello());
```

트랜스파일링을 실행해 보자. tsc 명령어 뒤에 트랜스파일링 대상 파일명을 지정한다. 이때 확장자 .ts는 생략할 수 있다.

```bash
$ tsc person
```

트랜스파일링 실행 결과, 같은 디렉토리에 자바스크립트 파일 person.js가 생성된다.

```javascript
var Person = /** @class */ (function () {
    function Person(name) {
        this.name = name;
    }
    Person.prototype.sayHi = function () {
        return "Hi, " + this.name;
    };
    return Person;
}());
var person = new Person('Kim');
console.log(person.sayHi());
```

이때 트랜스파일링된 person.js의 자바스크립트 버전은 ES3이다. 이는 TypeScript 컴파일 타겟 자바스크립트 기본 버전이 ES3이기 때문이다.

컴파일되는 자바스크립트 버전을 변경하려면 컴파일 옵션에 `--target` 또는 `-t`를 사용한다. 현재 tsc가 지원하는 자바스크립트 버전은 ES3(default), ES5, ES2015, ES2016, ES2017, ES2018, ES2019, ESNEXT이다. 만약 ES6 버전으로 트랜스파일링을 실행하고 싶다면 아래와 같이 컴파일 옵션을 추가한다.

```shell
$ tsc person -t ES2015
```

트랜스파일링이 성공하여 자바스크립트 파일이 생성되었으면 Node.js REPL을 이용해 트랜스파일링된 person.js를 실행할 수 있다.

```shell
$ node person
# Hi, Kim
```

> **Node.js REPL**
>
> an interactive shell that processes Node.js experssions.

매번 컴파일 옵션을 지정하는 것은 번거로우므로 tsc 옵션 설정 파일을 생성하도록 하자.

```shell
$ tsc --init
```

아래와 같이 tsc 옵션 설정 파일인 tsconfig.json이 생성된다.

```json
{
  "compilerOptions": {
    /* Basic Options */
    // "incremental": true,                   /* Enable incremental compilation */
    "target": "es5",                          /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
    "module": "commonjs",                     /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
```

tsc 명령어 뒤에 파일명을 지정하면 tsconfig.json이 무시된다.

```shell
$ tsc person # tsconfig.json이 무시된다.
```

tsconfig.json을 적용하려면 아래와 같이 트랜스파일링한다.

```shell
$ tsc
```

위와 같이 파일명을 지정하지 않으면 프로젝트 폴더 내의 모든 TypeScript 파일이 모두 트랜스파일링된다.

`---watch` 또는 `-w` 옵션을 사용하면 트랜스파일링 대상 파일의 내용이 변경되었을 때 이를 감지하여 자동으로 트랜스파일링이 실행된다.

```shell
$ tsc --watch
```

또는 아래와 같이 tsconfig.json에 watch 옵션을 추가할 수도 있다.

```json
{
    // ...
    "watch": true
}
```

컴파일러의 옵션에 대해서는 [TypeScript Compiler Options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)을 참조하길 바란다.

