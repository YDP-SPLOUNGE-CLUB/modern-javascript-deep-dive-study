# 21 빌트인 객체

## 21.1 자바스크립트 객체의 분류

1. 표준 빌트인 객체
   - 표준 빌트인 객체는 ECMAScript 사양에 정의된 객체를 말하며, 애플리케이션 전역의 공통 기능을 제공한다. 표준 빌트인 객체는 ECMAScript 사양에 정의된 객체이므로
   - 자바스크립트 실행 환경과 관계없이 사용할 수 있다.
2. 호스트 객체
    - 호스트 객체는 ECMAScript 사양에 정의되어 있지 않지만 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다.
3. 사용자 정의 객체
    - 사용자 정의 객체는 표준 빌트인 객체와 호스트 객체처럼 기본 제공되는 객체가 아닌 사용자가 직접 정의한 객체를 말한다.

## 21.2 표준 빌트인 객체

> 자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error
> 등 40여 개의 표준 빌트인 객체를 제공한다.

> Math, Reflect, JSON 을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.

> 예를 들어, 표준 빌트인 객체인 String, Number, Boolean, Function, Array, Date 는 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

```javascript
const strObj = new String('Lee');
console.log(typeof strObj); // object

const numObj = new Number('Lee');
console.log(typeof numObj); // object

const boolObj = new Boolean('Lee');
console.log(typeof boolObj); // object

const arr = new Array(1,2,3);
console.log(typeof arr); // object

const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object

const date = new Date();
console.log(typeof date); // object
```

> 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.
> 예를 들어, 표준 빌트인 객체인 String 을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype 이다.

````javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee');

console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
````

> 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체는 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.
> 그리고 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공한다.

> Number 의 prototype 프로퍼티에 바인딩된 객체, Number.prototype 은 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.
> 이 프로토타입 메서드는 모든 Number 인스턴스가 상속을 통해 사용할 수 있다. 

```javascript
// 생성자 함수에 의한 number 객체 생성
const numObj = new Number(1,5);

// toFixed 는 Number.prototype 의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2
```

## 21.3 원시값과 래퍼 객체

> 문자열이나 숫자, 불리언 등의 원시값이 있는데도 문자열, 숫자 불리언 객체를 생성하는 빌트인 생성자 함수가 존재하는 이유는 무엇일까?

```javascript
const str = 'hello';

console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

> 원시값인 문자열, 숫자, 불리언 값의 경우 접근시 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해 주기 떄문이다.
> 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.

> 이처럼 문자열, 숮자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 래퍼 객체라 한다.

> 래퍼 객체의 처리가 종료되면 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값으로 원래의 상태, 즉 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 콜렉션의 대상이 된다.

> 불리언 값도 문자열이나 숫자와 마찬가지이지만 불리언 값으로 메서드를 호출하는 경우는 없으므로 그다지 유용하지 않다.

> 따라서 String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열 숫자 불리언 인스턴스를 생성할 필요가 없으며 권장하지도 않는다.

## 21.4 전역 객체

> 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며
> 어떤 객체에도 속하지 않은 최상위 객체다.

> 브라우저 환경에서는 window 가 전역 객체를 가리키지만 Node.js 환경에서느 global 이 전역 객체를 가리킨다.

> 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체다.
> 전역 객체가 최상위 객체라는 것은 프로토타입 상속 관계상에서 최상위 객체라는 의미가 아니다.
> 전역 객체 자신은 어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.

> 전역 객체의 특징은 다음과 같다.

- 전역 객체는 개발자가 의도적으로 생성할 수 없다.
- 전역 객체의 프로퍼티를 참조할 때 window 를 생력할 수 있다.

```javascript
// 문자열 'F' 16진수를 해석하여 10진수로 변환 / 반환한다.
window.parseInt('F', 16); // 15

parseInt('F', 16); // 15

window.parseInt === parseInt // true
```

- 전역 객체는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.

### 21.4.1 빌트인 전역 프로퍼티

> 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미한다. 주로 애플리케이션 전역에서 사용하는 값을 제공한다.

### Infinity

```javascript
console.log(window.Infinity === Infinity); // true

console.log(3/0); // Infinity
```

### NaN, undefined

### 21.4.2 빌트인 전역 함수

> 빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

### eval

> eval 함수는 자바스크립트 코드를 나타내는 문자열을 인수로 전달받는다. 전달받은 문자열 코드가 표현식이라면 eval 함수는 문자열 코드를
> 런타임에 평가하여 값을 생성하고, 전달받은 인수가 표현식이 아닌 문이라면 eval 함수는 문자열 코드를 런타임에 실행된다. 문자열 코드가 여러 개의 문으로 이루어져 있다면 모든 문을 실행한다.


```javascript
eval('1 + 2'); //3
eval('var x = 5'); // undefined

console.log(x); // 5

const o = eval('({a: 1})');
console.log(o); // {a: 1}
```

> eval 함수는 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정한다.

```javascript
const x = 1;

function foo() {
    eval('var x = 2');
   console.log(x); // 2
}

foo();
console.log(x); // 1
```

> 위 예제의 eval 함수는 새로운 x 변수를 선언하면서 foo 함수 스코프에 선언된 x 변수를 동적으로 추가한다.
> 함수가 호출되면 런타임 이전에 먼저 함수 몸체 내부에 모든 선언문을 먼저 실행하고 그 결과를 스코프에 등록한다.
> eval 함수는 기존의 스코프를 런타임에 동적으로 수정한다. eval 함수에 전달된 코드는 이미 그 위치에 존재하던 코드처럼 동작한다.

> 단 strict mode 에서 eval 함수는 기존의 스코프를 수정하지 않고 eval 함수 자신의 자체적인 스코프를 생성한다.

> eval 함수를 통해 사용자로부터 입력받은 콘텐트를 실행하는 것은 보안에 매우 취약하다.
> 또한 엔진에 의해 최적화가 수행되지 않으므로 일반적인 코드의 실행에 비해 처리 속도가 느리다.

### isFinite

> 전달받은 인수가 정상적인 유한수인지 검사하여 유한수라면 true, 아니라면 false 반환한다.

### isNaN, parseFloat, parseInt

### encodeURI, decodeURI

> URI 를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다. URI는 인터넷에 있는 자원을 나타내는 유일한 주소를 말한다.

> 인코딩이란 URI의 문자들을 이스케이프 처리하는 것을 의미한다.
> 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것이다.

### encodeURIComponent, decodeURIComponent

> URI 구성 요소를 인수로 전달받아 인코딩한다. URI 의 문자들을 이스케이프 처리하는 것을 의미한다.

```javascript
const uriComp = 'name=이웅모&job=programmer&teacher';

// encodeURIComponent 함수는 인수로 전달받은 문자열을 URI 의 구성요소인 쿼리 스트링의 일부로 간주한다.
// 쿼리 스트링 구분자로 사용되는 = ? & 까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc); // name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec); // name=이웅모&job=programmer&teacher
```

### 21.4.3 암묵적 전역

```javascript
var x = 10;
function foo() {
    y = 20;
};

foo();

console.log(x + y); // 30

```

> y 변수의 선언을 찾을수 없으므로 참조 에러가 발생한다. 하지만 자바스크립트 엔진은
> y = 20 을 window.y = 20 으로 해석하여 전역 객체에 프로퍼티를 동적 생산한다. 이러한 현상을 암묵적 전역 이라한다.

> y 는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이다.
> 따라서 y는 변수가 아니다. 호이스팅 또한 발생하지 않는다.

```javascript
console.log(x); // undefined

console.log(y); // ReferenceError

var x = 10;

function foo() {
    y = 20;
}

foo();

console.log(x+y);
```

---

표준 빌트인 객체에 대해서 설명하시오.