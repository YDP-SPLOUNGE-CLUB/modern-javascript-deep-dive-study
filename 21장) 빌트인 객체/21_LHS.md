# 21.1 자바스크립트 객체의 분류

자바스크립트 객체는 크게 3개의 객체로 분류할 수 있다.
- 표준 빌트인 객체
    - ECMAScript 사양에 정의된 객체를 말한다.
    - 애플리케이션 전역의 공통 기능을 제공한다.
    - 자바스크립트 실행 환경 (브라우저 or Node.js)과 관계없이 사용할 수 있다.
    - 전역 객체의 프로퍼티로서 제공된다. 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.
- 호스트 객체
    - ECMAScript 사양에 정의되어 있지 않다.
    - 자바스크립트 실행 환경 (브라우저 or Node.js)에서 추가 제공하는 객체를 말한다.
    - 브라우저 : DOM, BOM, Canvas, XMLHttpRequest, fetch, 등..
    - Node.js : Node.js 고유 API를 호스트 객체로 제공
- 사용자 정의 객체
    - 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

# 21.2 표준 빌트인 객체

자바스크립트는 40여 개의 표준 빌트인 객체를 제공한다.
Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공하고
생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {'Lee'}
console.log(typeof strObj); // object

const numObj = new Number(123); // Number {123}
console.log(typeof numObj); // object

const boolObj = new Boolean(true); // Boolean {true}
console.log(typeof boolObj); // object

const func = new Function('x','return x * x'); // f anonymous(x)
console.log(typeof func); // function

const arr = new Array(1, 2, 3); // [1, 2, 3]
console.log(typeof arr); // object

const regExp = new RegExp(/ab_c/i); // /ab+c/i
console.log(typeof regExp); // object

const date = new Date();
console.log(typeof date); // object
```

표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩 된 객체다.
표준 빌트인 객체 String을 생성자 함수로서 호출하여 생성한 String인스턴스의 프로토타입은 String.prototype이다.

```js
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

# 21.3 원시값과 래퍼 객체
문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자, 불리언 객체를 생성하는 String, Number, Boolean 등의 표준 빌트인 생성자 함수가 존재하는 이유는 무엇일까??

원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없다.
하지만 원시값인 문자열이 마치 객체처럼 동작한다.

```js
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

이는 원시값에 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 때문이다.

즉, 원시값을 객체처럼 사용하면 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

이처럼 **문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체(wrapper object)** 라 한다.

```js
const str = 'hi';

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

1. 원시값을 마침표 표기법으로 접근한다.
2. 그 순간 래퍼 객체인 String생성자 함수의 인스턴스가 생성되고 그 문자열은 \[\[StringData]] 내부 슬롯에 할당된다.
3. 래퍼 객체의 처리가 종료되면 래퍼 객체의 \[\[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬랙션의 대상이 된다.

아래 예제를 통해 자세히 알아보자.
```js
// (1) 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// (2) 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체의 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// (3) 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 (2)에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬랙션의 대상이 된다.

// (4) 식별자 str은 새롭게 암묵적으로 생성된 ((2)에서 생성된 래퍼 객체와는 다른) 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined

// (5) 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 (4)에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬랙션의 대상이 된다.
console.log(typeof str, str); // string hello
```

이처럼 문자열, 숫자, 불리언, 심벌은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있으며 표준 빌트인 객체인 String, Number, Boolean, Symbol의 프로토타입 메서드 또는 프로퍼티를 참조할 수 있다.

따라서 String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지 않는다 Symbol은 생성자 함수가 아니므로 제외

**null과 undefined는 래퍼 객체를 생성하지 않는다.**

# 21.4 전역 객체
전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 **어떤 객체보다도 먼저 생성되는 특수한 객체**이며, **어떤 객체에도 속하지 않은 최상위 객체**다.

전역 객체는 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다.
브라우저 : window
Node.js: global

### globalThis
ECMAScript2020(ES11)에서 도입된 globalThis는 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다.
globalThis는 표준 사양이며 ECMAScript 표준 사양을 준수하는 모든 환경에서 사용할 수 있다.
```js
// 브라우저 환경
globalThis = this // true
globalThis = window // true
globalThis = self // true
globalThis = frames // true

// Node.js 환경 (12.0.0 이상)
globalThis = this // true
globalThis = global // true
```

전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체(표준 빌트인 객체와 호스트 객체)의 최상위 객체다.

**최상위 객체라는 것은 프로토타입 상속 관계상 최상위 객체라는 의미가 아니다.**

전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

**전역 객체의 특징**
- 전역 객체는 개발자가 의도적으로 생성할 수 없다. (즉, 생성자 함수 제공되지 않음)
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있다.
```js
window.parseInt('F', 16); // 15
// window 생략
parseInt('F', 16);
window.parseInt === parseInt; // true
```
- 전역 객체는 Object, String, Number, Boolean, Function, Array, RegExp, Date, Math, Promise 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다. (호스트 객체)
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역. 그리고 전역함수는 전역 객체의 프로퍼티가 된다.
```js
var foo = 1;
consolw.log(window.foo); //1

bar = 2;
console.log(window.bar); // 2

function baz() {return 3;}
console.log(window.baz()); //3
```
- let 이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. let, const 키워드로 선언한 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재하게 된다.
```js
let foo = 123;
console.log(window.foo); // undefined
```
- 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window를 공유한다.


## 21.4.1 빌트인 전역 프로퍼티
빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다.
( 자세한 내용은 책에 수록 )
- Infinity
- NaN
- undefined

## 21.4.2 빌트인 전역 함수
빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.
( 자세한 내용은 책에 수록 )
- eval
- isFinite
- isNaN
- parseFloat
- parseInt
- encodeURI / decodeURI
    -  = ? & 는 인코딩하지 않음
- encodeURIComponent / decodeURIComponent
    -  = ? & 까지 인코딩 한다.

### URI (Uniform Resource Identifier) 구분
https://www.mydomain.com:80/docs/search&category=javascript&lang=ko#intro
- URI : https://www.mydomain.com:80/docs/search&category=javascript&lang=ko#intro
- URL : https://www.mydomain.com:80/docs/search
- URN : www.mydomain.com:80/docs/search&category=javascript&lang=ko#intro

- Scheme(Protocol) : https
- Host(Domain) : www.mydomain.com
- Port : 80
- Path : /docs/search
- Query(QueryString) : category=javascript&lang=ko
- Fragment: \#intro

## 21.4.3 암묵적 전역
```js
var x = 10;

function foo() {
	//선언하지 않은 식별자에 값을 할당
	y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이다. 따라서 y는 변수가 아니다.
y는 변수가 아니므로 호이스팅이 발생하지 않는다.

```js
console.log(x); // undefined
console.log(y); // ReferenceError: y is not defined

var x = 10;
function foo() {
	y = 20;
}
foo();

console.log(x + y); // 30
```

변수가아닌 단지 프로퍼티인 y는 delete 연산자로 삭제할 수 잇다.
전역 변수는 프로퍼티이지만 delete연산자로 삭제할 수 없다.

```js
var x = 10;
function foo() {
	y = 20; // window.y = 20;
	console.log(x + y);
}
foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제되지 않는다.
delete y; // 프로퍼티는 삭제된다.

console.log(window.x); // 10
console.log(window.y); // undefined
```

# 알고 가기
---
- [ ] 표준 빌트인 객체란 무엇인가요?
- [ ] 호스트 객체란 무엇인가요?
- [ ] 사용자 정의 객체란 무엇인가요?
