객체는 상태(state)를 나타내는 프로퍼티와 동작(behavior)을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

동작을 나타내는 메서드는 자신이 속한 객체의 상태. 즉 프로퍼티를 참조하고 변경할 수 있어야 한다.
메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야한다.**

객체 리터럴 방식으로 생성한 객체는 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
const circle = {
	// 프로퍼티: 객체 고유의 상태 데이터
	radius: 5
	// 메서드: 상태 데이터를 참조하고 조작하는 동작
	getDiameter() {
		// 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
		// 자신이 속한 객체인 circle를 참조할 수 있어야 한다.
		return 2 * circle.radius;
	}
};

console.log(circle.getDiameter()); // 10
```

`getDiameter` 메서드는 메서드 자신이 속한 객체를 가리키는 식별자 `circle`을 참조하고 있다.
이 참조 표현식이 평가되는 시점은 `getDiameter`가 호출되어 함수 몸체가 실행되는 시점이다.

위에서 객체리터럴로 생성한 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다.
따라서 getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되어 `circle` 식별자에 할당된 이후다.
따라서 `circle` 식별자를 참조할 수 있다.

하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않고 바람직하지 않다.

this 키워드를 통해 자신이 속한 객체 또는 자신이 생성 할 인스턴스를 가리킬 수 있다.

### this
>this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
>this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되며 코드 어디서든 참조할 수 있다.

함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역 변수처럼 사용할 수 있다.

단, **this가 가리키는 값. this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

### this 바인딩
> 바인딩이란 식별자와 값을 연결하는 과정을 의미한다.
> 예를 들어 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다.
> this바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 바인딩하는 것이다.

위의 예제를 this로 변경해보자.

```js
// 생성자
function Circle(radius) {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
}
Circle.prototype.getDiameter = function () {
	// this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

**자바스크립트의 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값, 즉 this 바인딩이 동적으로 결정된다.**
엄격 모드(strictmod) 역시 this바인딩에 영향을 준다.

this는 코드 어디에서나 참조 가능하다 따라서 함수 내부에서도 참조할 수 있다.
```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
	// 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
	console.log(this); // window
	return number * number;
}
square(2);

const person = {
	name: 'Lee',
	getName() {
		// 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
		console.log(this); // {name: Lee, getName: f}
		return this.name;
	}
}
console.log(person.getName()); // Lee

function Person(name) {
	this.name = name;
	// 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	console.log(this); // Person {name: 'Lee'}
}
const me = new Person('Lee')
```

**this는 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.**
strictmode가 적용된 일반 함수 내부의 this에서는 undefined가 바인딩된다.
일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 22.2 함수 호출 방식과 this 바인딩
**this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.**
### 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.
> 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다.

함수의 호출 방식
- 일반 함수 호출
- 메서드 호출
- 생성자 함수 호출
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
	console.dir(this);
}

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = {foo};
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = {name: 'bar'};

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar); // bar
```

## 22.2.1 일반 함수 호출
기본적으로 this에는 전역 객체가 바인딩 된다.
```js
function foo() {
	console.log(this); // window (strictmode일 경우 undefined)
	function bar() {
		console.log(this); // window(strictmode일 경우 undefined)
	}
	bar();
}
foo();
```

전역 함수는 물론이고 **중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.**

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인됭된다.
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // {value: 100, foo: f}
		console.log(this.value); // 100

		// 메서드 내에서 정의한 중첩 함수
		function bar() {
			console.log(this); // window
			console.log(this.value); // 1
		}
        // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩함수 내부의 this 에는
        // 전역 객체가 바인딩된다.
        bar();
	}
}

obj.foo();
```

콜백 함수가 일반함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.
어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		console.log(this); // {value: 100, foo: f}
		console.log(this.value); // 100

		// 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
		setTimeout(function bar() {
			console.log(this); // window
			console.log(this.value); // 1
		}, 100)
	}
}

obj.foo();
```
#중첩함수 #콜백함수
**이처럼 일반 함수로 호출된 모든 함수(중첩함수, 콜백함수) 내부의 this에는 전역 객체가 바인딩된다.**

메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		// this 바인딩(obj)을 변수 that에 할당
		const that = this;

		// 콜백 함수 내부에서 this 대신 that을 참조한다.
		setTimeout(function() {
			console.log(that.value); // 100
		}, 100)
	}
};

obj.foo();
```

이 방법 이외에도 apply, call, bind 메서드를 통해서도 가능하다.
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		// 콜백 함수에 명시적으로 this를 바인딩한다.
		setTimeout(function() {
			console.log(that.value); // 100
		}.bind(this), 100)
	}
};

obj.foo();
```

또는 화살표 함수를 사용해서 this 바인딩을 일치시킬 수도 있다.
```js
var value = 1;

const obj = {
	value: 100,
	foo() {
		// 콜백 함수 내부에서 this 대신 that을 참조한다.
		setTimeout(() => console.log(that.value), 100); // 100
	}
};

obj.foo();
```

## 22.2.2 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.

주의할 것은 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

```js
const person = {
	name: 'Lee',
	getName() {
		// 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
		return this.name;
	}
};
// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

`person` 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 `person` 객체에 포함된 것이 아니라
독립적으로 존재하는 별도의 객체다.

`getName` 프로퍼티가 함수 객체를가리키고 있을 뿐이다.
즉, `getName` 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고
일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```js
const anotherPerson = {
	name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
// this.name === window.name
console.log(getName); // ''

// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''다.
// Node.js 환경에서 this.name은 undefined다.
```

따라서 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩된다.

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```js
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function () {
	return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
// getName 메서드 내부의 this는 me를 가리키며 this.name 은 Lee다
console.log(me.getName()); // Lee

Person.prototoype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
// Person.prototype도 객체이므로 직접 메서드를 호출할 수 있다.
// getName 메서드 내부의 this는 Person.prototype을 가리키며 this.name은 Kim 이다.
console.log(Person.prototype.getName()); // Kim (2)
```

## 22.2.3 생성자 함수 호출
생성자 함수 내부의 this에는 생성자 함수가(미래에) 생성할 인스턴스가 바인딩된다.

```js
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
만약 new 연산자를 생략하면 일반함수로 동작한다.

```js
const circle3 = Circle(15);
// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3) // undefined
// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // radius
```

## 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

```js
function getThisBinding() {
	return this;
}

// this로 사용할 객체
const thisArg = { a:1 };

console.log(getThisBinding()); // window

// 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.
두 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.

```js
function getThisBinding() {
	console.log(arguments);
	return this;
}

// this로 사용할 객체
const thisArg = { a:1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol. iterator): f]
// {a: 1}

console.log(getThisBinding.call(thisArg, 1, 2, 3));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol. iterator): f]
// {a: 1}
```

apply 와 call 메서드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다.
arguments 객체는 배열이 아니기 때문에 Array.prototype 메서드를 사용할 수 없지만 apply call을 이용하면 사용할 수 있다.

```js
function convertArgsToArray() {
	console.log(arguments);
	const arr = Array.prototype.slice.call(arguments);
	console.log(arr);
	return arr;
};

convertArgsToArray(1,2,3); // [1, 2, 3]
```

Function.prototype.bind 메서드는 apply와 call 메서드와 달리 함수를 호출하지 않는다.
다만 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```js
function getThisBinding() {
	return this;
}

const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

bind 메서드는 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

```js
const person = {
	name: 'Lee',
	foo(callback) {
		setTimeout(callback, 100);
	}
};

person.foo(function() {
	// (this.name === window.name)window.name은 기본 ''다
	console.log(this.name); // ''
})
```

콜백 함수의 내부의 this를 외부 함수 내부의 this와 일치시키기 위해 bind 메서드를 사용하여 this를 일치시킨다.
```js
const person = {
	name: 'Lee',
	foo(callback) {
		// bind 메서드로 callback 함수 내부의 this 바인딩을 전달
		setTimeout(callback.bind(this), 100);
	}
};

person.foo(function() {
	console.log(this.name); // Lee
})
```

### 정리
| 함수 호출 방식                                             | this 바인딩                                        |
| ---------------------------------------------------------- | -------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                          |
| 메서드 호출                                                | 메서드를 호출한 객체                               |
| 생성자 함수 호출                                           | 생성자 함수가(미래에)생성할 인스턴스               |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |

# 알고 가기
---
- [ ] this란 무엇인지 설명해주세요.
- [ ] apply, call, bind 에 대해 설명해주세요.
