# 18 함수와 일급 객체

## 18.1 일급 객체

1. 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
2. 변수나 자료구조에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

> 자바스크립트의 함수는 다음 예제와 같이 위의 조건을 모두 만족하므로 일급 객체다.

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

> 함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미다.
> 객체는 값이므로 함수는 값과 동일하게 취급할 수 있다. 따라서 함수는 값을 사용할 수 있는 곳이라면 어디서든지 리터럴로 정의할 수 있으며
> 런타임에 함수 객체로 평가된다.

> 일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로
> 사용할 수 있다는 것이다. 이는 함수형 프로그래밍을 가능케 하는 자바스크립트의 장점 중 하나다.

## 18.2 함수 객체의 프로퍼티

> 함수는 객체다. 따라서 함수도 프로퍼티를 가질 수 있다.

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

> 이처럼 arguments, caller, name, prototype 프로퍼티는 모두 함수 객체의 데이터 프로퍼티다. 이들 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티다.
> 하지만 __proto__ 는 접근자 프로퍼티이며, 함수 객체 고유의 프로퍼티가 아니라 Object.prototype 객체의 프로퍼티를 상속받은 것을 알 수 있다.

> Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다. 즉 Object.prototype 객체의 __proto__ 접근자 프로퍼티는 모든 객체가 사용할 수 있다.

### 18.2.1 argument 프로퍼티

> 함수 객체의 argument 프로퍼티 값은 argument 객체다. argument 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며
> 함수 내부에서 지역 변수처럼 사용된다. 즉 함수 외부에서는 참조할 수 없다.

> 함수 객체의 argument 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만 ES3 부터 표준에서 폐지되었다.
> Function.arguments 와 같은 사용법은 권장되지 않는다. 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조하도록 한다.

> 자바스크립트는 함수의 매개변수와 인수의 개수가 일치하는지 확인하지 않는다. 따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.

> 초과된 인수가 버려지는 것은 아니고 arguments 객체의 프로퍼티로 보관된다.

> 선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 때문에
> 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수의 동작을 달리 정의할 필요가 있을 수 있다.
> 이 떄 유용하게 사용되는 것이 arguments 객체다.

> arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.


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

> arguments 객체는 배열 형태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체다.

> 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다.
> 따라서 배열 메서드를 사용하려면 Function.prototype.call, apply 를 사용해 간접 호출해야 하는 번거로움이 있다.

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

### 18.2.2 caller 프로퍼티

> caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다. 관심이 없다면 지나쳐도좋다.

네! 지나치도록 하겠습니다.

### 18.2.3 length 프로퍼티

> 함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

### 18.2.4 name 프로퍼티

> 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다. name 프로퍼티는 ES6 이전까지는 비표준이었다가 ES6 에서 정식 표준이 되었다.

> name 프로퍼티는 ES5 와 ES6 에서 동작을 달리하므로 주의하길 바란다. 익명 함수 표현식의 경우 ES5 에서 name 프로퍼티는 빈 문자열을 값으로 가진다.
> 하지만 ES6 에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

### 18.2.5 __proto__ 접근자 프로퍼티

> 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 갖는다. 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

> __proto__ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 상요하는 접근자 프로퍼티다.

> 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.

```js
const obj = {a: 1}

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype 이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

### hasOwnProperty 메서드
> hasOwnProperty 메서드는 이름에서 알 수 있듯이 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true 를 반환한다.
> 프로토타입의 프로퍼티 키 값이라면 false 를 반환한다.

### 18.2.6 prototype 프로퍼티

> prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티다.

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function(){}).hasOwnProperty('prototype'); // true
// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // false
```
