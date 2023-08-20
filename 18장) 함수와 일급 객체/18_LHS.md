# 18.1 일급 객체

일급 객체란 다음 조건을 만족하는 객체를 말한다.
1. 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

다음 예제는 모두 일급 객체다.
```js
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
// 런타임(할당 단계)에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
	return ++num;
}
const decrease = function (num) {
	return --num;
}

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
	let num = 0;
	return function () {
		num = aux(num);
		return num;
	};
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(auxs.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미
객체는 값이므로 함수는 값과 동일하게 취급할 수 있다.

다라서 함수는 값을 사용할 수 있는 곳
(변수 할당문, 객체 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문) 이라면
어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가된다.
이와 같은 일급 객체의 특징은 함수형 프로그래밍을 가능케 하는 자바스크립트의 장점 중 하나다.
# 18.2 함수 객체의 프로퍼티
함수는 객체다.
함수 객체의 내부를 들여다보자.

```js
function square(number) {
	return number * number;
}
console.dir(square);
console.log(Object.getOwnPropertyDescriptors(square));

// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, '__proto__')); // undefined
// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'))
// { get: f, set: f, enumerable: false, configurable: true}
```

arguments, caller, length, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다.
데이터 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.

\_\_proto\_\_는 접근자 프로퍼티이며 함수 객체 고유의 프로퍼티가 아니라
Object.property 객체의 프로퍼티를 상속받은 것을 알 수 있다.

즉 Object.prototype 객체의 \_\_proto\_\_ 접근자 프로퍼티는 모든 객체가 사용할 수 있다.

## 18.2.1 arguments 프로퍼티
함수 객체의 arguments 프로퍼티 값은 arguments 객체다.
arguments 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한(iterable) 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.

즉 함수 외부에서는 참조할 수 없다.

자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
```js
function multiply(x, y) {
	console.log(arguments);
	return x * y;
}

console.log(multiply()); // NaN
console.log(multiply(1)); // NaN
console.log(multiply(1, 2)); // 2
console.log(multiply(1, 2, 3)); // 2

```

함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급된다.
함수가 호출되면 함수 몸체 내에 암묵적으로 매개변수가 선언되고 undefined로 초기화된 후 인수가 할당된다.

매개변수 개수보다 인수를 적개 전달했을 경우 전달받지 못한 매개변수는 undefined로 초기화된 상태를 유지한다. 또한 초과된 인수는 무시된다.

그렇다고 초과된 인수가 버려지는 것은 아니고
모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.

### arguments 객체의 Symbol(Symbol.iterator) 프로퍼티
>arguments 객체의 Symbol(Symbol.iterator) 프로퍼티는 arguments 객체를 순회 가능한 자료구조인 이터러블로 만들기 위한 프로퍼티다. Symbol.iterator를 프로퍼티 키로 사용한 메서드를 구현하는 것에 의해 이터러블 된다.


arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할때 유용하다.

```js
function sum() {
	let res = 0;
	for (let i = 0; i < arguments.length; i++) {
		res += arguments[i];
	}
	return res;
}
console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

argments 객체는 배열 형태로 인자 정보를 담고있지만
실제 배열이 아닌 **유사 배열 객체**다.

### 유사배열 객체와 이터러블
> ES6에 도입된 이터레이션 프로토콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다.
> 이터러블의 개념이 없었던 ES5에서 argumnets 객체는 유사 배열 객체로 구분되었다.
> 하지만 이터러블이 도입된 ES6부터 arguments 객체는 유사 배열 객체이면서 동시에 이터러블이다.


유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 때 에러가 발생한다.
배열 메서드를 사용하려면 간접 호출해야 하는 번거로움이 있다.

```js
function sum() {
	const array = Array.prototype.slice.call(arguments);
	return array.reduce(function (pre, cur) {
		return pre + cur;
	}, 0);
}

// ES6에 도입된 Res 파라미터
function sum(...args) {
	return args.reduce((pre, cur) => pre + cur, 0);
}
```

## 18.2.2 caller 프로퍼티
caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다.
표준화될 예정도 없기 때문에 사용하지 말고 참고로만 알아둘 것

caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.
```js
function foo(func) {
	return func();
}
function bar() {
	return 'caller: ' + bar.caller;
}

// 브라우저 실행 결과
console.log(foo(bar)); // caller: function foo(func) {...}
console.log(bar); // caller: null
```

Node.js 환경에서는 다른 결과가 나온다.

## 18.2.3 length 프로퍼티
함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

```js
function foo() {}
console.log(foo.length); // 0

function bar(x) {
	return x;
}
console.log(bar.length); // 1

function baz(x, y) {
	return x * y
};
console.log(baz.length); // 2
```

arguments 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티 값은 다를 수 있으므로 주의하자.
arguments 객체의 length 프로퍼티는 인자의 개수를 가리키고
함수 객체의 length 프로퍼티는 매개 변수의 개수를 가리킨다.

## 18.2.4 name 프로퍼티
함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.
ES6에 정식 표준이 되었다.

익명함수 표현식의 경우 ES5와 ES6의 동작이 다르다.
ES5의 name 프로퍼티는 빈 문자열을 값으로 갖고
ES6의 name 프로퍼티는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```js
// 기명 함수 표현식
var namedFunc = function foo() {};
console.log(namedFunc.name); //foo

// 익명함수 표현식
var anonymousFunc = function() {}
// ES5: 빈 문자열
// ES6: 함수 객체를 가리키는 변수 이름을 값으로 갖는다.
console.log(anonymousFunc.name); // anonymousFunc

// 함수 선언문
function bar() {}
console.log(bar.name); // bar
```

함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출한다.

## 18.2.5 \_\_proto\_\_ 접근자 프로퍼티

모든 객체는 \[\[Prototype]] 이라는 내부 슬롯을 갖는다.
\[\[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

\_\_proto\_\_ 프로퍼티는 \[\[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다.

```js
const obj = {a: 1}

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype 이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('a')); // true
console.log(ojb.hasOwnProperty('__proto__')); // false
```

### hasOwnProperty 메서드
> 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.


## 18.2.6 prototype 프로퍼티
prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체
즉 constructor만이 소유하는 프로퍼티다.
일반 객체와 생성자 함수로 호출할 수 없는 non-constructor에는 prototype 프로퍼티가 없다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function(){}).hasOwnProperty('prototype'); // true
// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // false
```
prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

# 알고 가기
---
- [ ] 함수 객체의 데이터 프로퍼티인 arguments, caller, length, name, prototype 에 대해 설명해주세요.
