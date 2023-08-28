자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍(OOP: Object Ori-ented Programming)을 지원하는 멀티 패러다임 프로그래밍이다.

자바스크립트는 public, private, protected등이 없어서 객체지향 언어가 아니라고 오해하는 경우도 있지만
클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 프로토타입 기반의 객체지향 프로그래밍 언어다.

### 클래스
> ES6에서 클래스가 도입되었다.
> ES6의 클래스가 기존 프로토타입 기반 객체지향 모델을 폐지하고 새로운 객체지향 모델을 제공하는 것은 아니다.
> 사실 클래스도 함수이며 기존 프로토타입 기반 패턴의 문법적 설탕이라고 볼 수 있다.
> 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.
> 클래스는 생성자 함수보다 엄격하며 클래스는 생성자 함수에서는 제공하지 않는 기능도 제공한다.
> 클래스를 프로토타입 기반 객체 생성 패턴의 단순한 문법적 설탕으로 보기보다는 새로운객체 생성 메커니즘으로 보는 것이 좀 더 합당하다고 할 수 있다.


자바스크립트는 객체 기반의 프로그래밍 언어이며
**자바스크립트를 이루고 있는 거의 "모든 것"이 객체다.**

# 19.1 객체지향 프로그래밍

객체지향 프로그래밍은 프로그램을 명령어 또는 함수 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

객체지향 프로그래밍은 실세계의 실체(사물이나 개념)를 인식하는 철학적 사로를 프로그래밍에 접목하려는 시도에서 시작한다.
실체는 특징이나 성질을 나타내는 속성(attribute/property)를 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 추상화라 한다.

```js
// 이름과 주소 속성을 갖는 객체
const person = {
	name: 'Lee',
	address: 'Seoul'
};
```

### 객체
- 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조
- **상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조**
- **상태**를 프로퍼티(property) **동작**을 메서드(method) 라고 부른다.
### 객체지향 프로그래밍
- 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 객체의 **상태**를 나타내는 데이터와 상태를 조작할 수 있는 **동작**을 하나의 논리적인 단위로 묶어서 생각

# 19.2 상속과 프로토타입

### 상속 (inheritance)
객체지향 프로그래밍의 핵심 개념으로 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

```js
function Circle(radius) {
	this.radius = radius;
	this.getArea = function () {
		return Math.PI * this.radius ** 2;
	};
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

/*
* Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
* getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
* getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
*/
console.log(circle1.getArea == circle2.getArea); // false
console.log(circle1.getArea()); // 3.141592
console.log(circle2.getArea()); // 12.56637
```
위 예제의 Circle 생성자 함수는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.

모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필요하게 낭비한다.
10개의 인스턴스를 생성하면 내용이 동일한 메서드도 10개 생성되는 것이다.

상속을 통해 불필요한 중복을 제거하면 아래와 같다.
**자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

```js
function Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

/*
* Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
* 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
* 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
*/
console.log(circle1.getArea == circle2.getArea); // true
console.log(circle1.getArea()); // 3.141592
console.log(circle2.getArea()); // 12.56637
```

상속은 코드의 재사용이란 관점에서 매우 유용하다.

# 19.3 프로토타입 객체
프로토타입 객체(또는 프로토타입)란
객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.
### 프로토타입
> 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다.
> 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

모든 객체는 \[\[Prototype]] 이라는 내부 슬롯을 가지며 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.
즉 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 \[\[Prototype]]에 저장된다.

**객체 리터럴에 의해 생성된 객체**
- 프로토타입은 Object.prototype 이다.
  **생성자 함수에 의해 생성된 객체**
- 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

모든 객체는 하나의 프로토타입을 가지며 모든 프로토타입은 생성자 함수와 연결되어 있다.
즉, 객체와 프로토타입 생성자 함수는 연결되어있다.

\[\[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만 \_\_proto\_\_ 접근자 프로퍼티를 통해 자신의 프로토타입 즉 자신의 \[\[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다.

프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고
생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

## 19.3.1 \_\_proto\_\_  접근자 프로퍼티
**모든 객체는 \_\_proto\_\_  접근자 프로퍼티를 통해 자신의 프로토타입, \[\[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.**

```js
const person = {name: 'Lee'};

console.log(person);
console.log(person.__proto__);
```

### \_\_proto\_\_ 는 접근자 프로퍼티다.
내부 슬롯은 프로퍼티가 아니다.
\[\[Prototype]] 내부 슬롯에도 직접 접근할 수 없기에 \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 \[\[Prototype]] 내부 슬롯의 값, 즉 프로퍼티에 접근할 수 있다.

```js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter 함수인 set __proto__ 가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

### \_\_proto\_\_  접근자 프로퍼티는 상속을 통해 사용된다.
\_\_proto\_\_  접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해 Object.prototype.\_\_proto\_\_  접근자 프로퍼티를 사용할 수 있다.

```js
const person = {name: 'Lee'};

console.log(person.hasOwnProperty('__proto__')); // false
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

### Object.prototype
> 모든 객체는 프로토타입의 계층 구조인 프로토타입 체인에 묶여있다. 자바스크립트 엔진은 객체의 프로퍼티에 접근하려고 할때 해당 객체에 접근하려는 프로퍼티가 없다면 \_\_proto\_\_ 접근자 프로퍼티가 가리키는 참조를 따라 자신의 부모역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
> 프로토타입 체인의 최상위 객체는 Object.prototype이며 이 객체의 프로퍼티와 메서드는 모든 객체에 상속된다.


### \_\_proto\_\_  접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
\[\[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```js
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.
즉 검색 방향이 한쪽 방향으로만 흘러가야 한다.
따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 \_\_proto\_\_  접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

### \_\_proto\_\_  접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

\_\_proto\_\_  접근자 프로퍼티는 ES5까지 ECMAScript 사양에 포함되지 않은 비표준이었다.
브라우저 호환성을 고려하여 ES6에서 \_\_proto\_\_ 를 표준으로 채택했다.

코드 내에서 \_\_proto\_\_ 를 직접 사용하는 것은 권장하지 않는다.
모든 객체가 \_\_proto\_\_  접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다.

직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 \_\_proto\_\_  접근자 프로퍼티를 사용할 수 없는 경우가 있다.

```js
// obj 는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속 받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

\_\_proto\_\_  접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는
**Object.getPrototypeOf 메서드**를 사용하고
프로토타입을 교체하고 싶은 경우에는
**Object.setPrototypeOf 메서드**를 사용하는 것을 권장한다.

```js
const obj = {};
const parent = { x: 1 };

Object.getPrototypeOf(obj); // obj.__proto__;
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

## 19.3.2 함수 객체의 prototype 프로퍼티
**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

```js
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function (){}).hasOwnProperty('prototype'); // true
// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // false
```

prototype 프로퍼티는 생성자 함수가 생성할 객체(인스턴스)의 프로토타입을 가리킨다.
즉 생성자 함수로서 호출할 수 없는 함수 non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

```js
const Person = name => {
	this.name = name;
};

console.log(Person.hasOwnProperty('prototype')); // false
console.log(Person.prototype); // undefined

const obj = {
	foo() {}
};

console.log(obj.foo.hasOwnProperty('prototype')); // false
console.log(obj.foo.prototype); // undefined
```

객체 생성을 하지 않는 일반 함수의 prototype 프로퍼티는 아무런 의미가 없다.

**모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) \_\_proto\_\_  접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**

하지만 이들 프로퍼티를 사용하는 주체가 다르다.

| 구분                          | 소유       | 값                | 사용 주체   | 사용 목적                                                                    |
| ----------------------------- | ---------- | ----------------- | ----------- | ---------------------------------------------------------------------------- |
| \_\_proto\_\_ 접근자 프로퍼티 | 모든 객체  | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용                      |
| prototype 프로퍼티            | construtor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

```js
function Person(name) {
	this.name = name;
}
const me = new Person('Lee');

console.log(Person.prototype === me.__proto__); // true
```

## 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수
모든 프로토타입은 constructor 프로퍼티를 갖는다.
```js
function Person(name) {
	this.name = name;
}
const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

me 객체에 constructor 프로퍼티가 없지만 me 객체의 프로토타입인 Person.prototype에는 constructor 프로퍼티가 있다.
따라서 me 객체는 프로토타입인 Person.prototype의 constructor 프로퍼티를 상속받아 사용할 수 있다.

# 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

```js
const obj = new Object();
console.log(obj.constructor === Object); // true

const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

function Person(name) {
	this.name = name;
}

const me = new Person('Lee');
console.log(me.constructor === Person); // true
```

리터럴 표기법에 의한 객체 생성방식과 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```js
// 객체 리터럴
const obj = {};
// 함수 리터럴
const add = function (a, b) { return a + b; };
// 배열 리터럴
const arr = [1, 2, 3];
// 정규 표현식 리터럴
const regexp = /is/ig;
```

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입이 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

```js
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true

// Object 생성자 함수에 의한 객체 생성
// 인수가 전달되지 않았을 때 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성한다.
let obj2 = new Object();
console.log(obj2); // {}

// new.target이 undefined나 Object가 아닌 경우
// 인스턴스 -> Foo.prototype -> Object.prototype 순으로 프로토타입 체인이 생성된다.
class Foo extends Object {}
new Foo(); // Foo {}

// 인수가 전달된 경우에는 인수를 객체로 변환한다.
// Number 객체 생성
obj = new Object(123);
console.log(obj); // Number {123}

// String 객체 생성
obj = new Object('123');
console.log(obj); // String {"123"}
```

객체 리터럴이 평가될 때는 다음과 같이 추상연산 OrdinaryObjectCreate 를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.

### 추상 연산
> ECMAScript 사양에서 내부 동작의 구현 알고리즘을 표현한 것이다.
> ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해하자.

Object 생성자 함수 호출과 객체 리터럴의 평가는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하는 점에서 동일하나 new.target의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다.
**따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.**

함수 객체의 경우 차이가 명확하다.
```js
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다.
리터럴 표기법에 의해 생성된 객체도 가상의 생성자 함수를 갖는다.
프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문.

**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

| 리터럴 표기법      | 생성자 함수 | 프로토타입         |
| ------------------ | ----------- | ------------------ |
| 객체 리터럴        | Object      | Object.prototype   |
| 함수 리터럴        | Function    | Function.prototype |
| 배열 리터럴        | Array       | Array.prototype    |
| 정규 표현식 리터럴 | RegExp      | RegExp.prototype   |

# 19.5 프로토타입의 생성 시점

객체는 객체 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

**프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**
- 생성자 함수는 단독으로 존재할 수 없도 언제나 쌍으로 존재하기 때문

생성자 함수의 구분
- 사용자가 직접 정의한 사용자 정의 생성자 함수
- 자바스크립트가 제공하는 빌트인 생성자 함수

## 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점
생성자 함수로서 호출할 수 있는 함수
**즉, constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**

```js
// 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
console.log(Person.prototype); // {constructor: f}

// 생성자 함수
function Person(name) {
	this.name = name;
}

// 화살표 함수
const Person = name => {
	this.name = name;
}
console.log(Person.prototype); // undefined
```

이처럼 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며
생성된 프로토타입의 프로토타입은 언제나 Object.prototype 이다.

## 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점
Object, String, Number, Function, Array, RegExp, Date, Promise 등 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자가 생성되는 시점에 프로토타입이 생성된다.

모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.
생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

### 전역 객체
전역 객체는 코드가 실행되기 이전 단계에서 자바스크립트 엔진에 의해 생성되는 특수한 객체다.
전역 객체는 클라이언트 사이드 환경(브라우저)에서 window
서버 사이드 환경(Node.js)에서는 global 객체를 의미한다.

전역객체는 표준 빌트인 객체 (Object, String, Number, ...) 들과 환경에 따른 호스트 객체(클라이언트 Web API 또는 Node.js의 호스트 API) 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.
Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 생성자 함수다.
```js
window.Object === Object // true
```


이처럼 객체가 생성되기 이전에 함수와 프로토타입은 객체화되어 존재한다.

**이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 \[\[Prototype]]내부 슬롯에 할당된다.**

# 19.6 객체 생성 방식과 프로토타입의 결정

객체의 생성 방식은
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)
  이처럼 다양한 방식으로 생성된 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

OridinaryObjectCreate
- 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 받는다.
- 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달할 수 있다.
- 빈 객체를 생성한 후, 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가한다.
- 인수로 전달받은 프로토타입을 자신이 생성한 객체의 \[\[Prototype]] 내부 슬롯에 할당한 다음 생성한 객체를 반환한다.

즉 프로토타입은 추상연산 OridinaryObjectCreate에 전달되는 인수에 의해 결정됨.

## 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```js
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```


## 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.
객체 리터럴에 의해 생성된 객체와 동일한 구조를 갖는다.

```js
const obj = new Object();
obj.x = 1;

// Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

객체 리터럴과 Object 생성자 함수에 의한 **차이점**은 프로퍼티를 추가하는 방식에 있다.

**객체 리터럴 방식**
- 객체 리터럴 내부에 프로퍼티를 추가
  **Object 생성자 함수 방식**
- 빈 객체를 생성한 이후 프로퍼티를 추가

## 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

```js
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');
```

표준 빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드(hasOwnProperty, propertyIsEnumerable 등)를 갖고있다.
하지만 사용자 정의 생성 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor뿐이다.

# 19.7 프로토타입 체인

```js
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`hi ${this.name}`);
};

const me = new Person('Lee');

console.log(me.hasOwnProperty('name')); // true
```

```js
Object.getPrototypeOf(me) === Person.prototype; // true
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
```

me 객체가 Person.prototype 뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.
me 객체의 프로토타입은 Person.prototype이다.

### 프로토타입 체인
#프로토타입체인
>자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면
>\[\[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
>프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

```js
// hasOwnProperty는 Object.prototype의 메서드다.
// me 객체는 프로토타입 체인을 따라 hasOwnProperty 메서들를 검색하여 사용한다.
me.hasOwnProperty('name'); // true
```

검색 과정
1. hasOwnProperty 메서드를 호출한 me 객체에서 hasOwnProperty 메서드를 검색
   hasOwnProperty 가 없으므로 프로토타입 체인, 즉 \[\[Prototype]] 내부 슬롯에 바인딩되어 있는 프로토타입 으로 이동하여 hasOwnProperty 메서드를 검색한다.
2. Person.prototype에도 hasOwnProperty 메서드가 없으므로 프로토타입으로 이동하여 hasOwnProperty 메서드를 검색한다.
3. Object.prototype에는 hasOwnProperty 메서드가 존재한다.
   자바스크립트 엔진은 Object.prototype.hasOwnProperty 메서드를 호출한다. 이때 Object.prototype.hasOwnProperty 메서드의 this에는 me 객체가 바인딩된다.

```js
Object.prototype.hasOwnProperty.call(me, 'name');
```

**프로토타입 체인의 최상단에 위치하는 객체는 언제나 Object.prototype이다.**
Object.prototype을 프로토타입 체인의 종점 (end of prototype chain)이라 한다.

Object.prototype 에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다.

자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티/메서드를 검색한다.
- 객체 간의 상속 관계로 이루어진 프로토타입의 계층적인 구조에서 객체의 프로퍼티를 검색한다.
- 즉 **프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘** 이라고 할 수 있다.

프로퍼티가 아닌 식별자는 스코프 체인에서 검색한다.
- 함수의 중첩 관계로 이루어진 스코프의 계층적 구조에서 식별자를 검색한다.
- 즉 **스코프 체인은 식별자 검색을 위한 메커니즘**이라고 할 수 있다.

### 정리
**스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.**

# 19.8 오버라이딩과 프로퍼티 섀도잉

```js
const Person = (function () {
	// 생성자 함수
	function Person(name) {
		this.name = name;
	}
	// 프로토타입 메서드
	Person.prototype.sayHello = function () {
		console.log(`Hi ${this.name}`);
	};
	// 생성자 함수를 반환
	return Person;
}());

const me = new Person('Lee');

// 인스턴스 메서드
me.sayHello = function () {
	console.log(`Hey ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey Lee
```

생성자 함수로 객체(인스턴스)를 생성한 다음,
인스턴스에 메서드를 추가했다.

프로토타입이 소유한 프로퍼티(메서드포함)을 **프로토타입 프로퍼티**
인스턴스가 소유한 프로퍼티를 **인스턴스 프로퍼티** 라고 부른다.

인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고
프로토타입 메서드 sayHello는 가려진다.

이처럼 상속 관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티섀도잉 이라 한다.

## 오버라이딩 과 오버로딩
### 오버라이딩
> 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
### 오버로딩
> 함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식이다. 자바스크립트는 오버로딩을 지원하지 않지만 argouments 객체를 사용하여 구현할 수 는 있다.

프로퍼티를 삭제하는 경우도 마찬가지다.
```js
// 인스턴스 메서드를 삭제한다.
delete me.sayHello;
// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi Lee

// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
me.sayHello(); // Hi Lee
```

하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다.
하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않는다.

프로토타입 프로퍼티를 변경, 삭제하려면 프로토타입에 직접 접근해야한다.

```js
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
	console.log(`Hey ${this.name}`);
};
me.sayHello(); // Hey Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError
```


# 19.9 프로토타입 교체
프로토타입은 임의의 대란객체로 변경할 수 있다.
부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미

이러한 특징을 활용해 객체 간의 상속 관계를 동적으로 변경할 수 있다.

## 19.9.1 생성자 함수에 의한 프로토타입 교체
```js
const Person = (function () {
	function Person(name) {
		this.name = name;
	}
	// (1) 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		sayHello() {
			console.log(`Hi ${this.name}`)
		}
	};
	return Person;
}());

const me = new Person('Lee');
```

(1) 에서 객체 리터럴을 할당 했는데 이는 Person 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체한 것이다.

교체한 객체 리터럴에는 constructor 프로퍼티가 없다.
하지만 엔진에 의해 암묵적으로 constructor가 추가된다.
따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

```js
// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
console.log(me.constructor === Person); // false
// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색됨.
console.log(me.constructor === Object); // true
```

이처럼 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.
아래와 같이 파괴된 constructor 프로퍼티와 생성자 함수 간의 연결을 되살릴 수 있다.

```js
const Person = (function () {
	function Person(name) {
		this.name = name;
	}
	// 생성자 함수의 pototoype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		constructor: Person,
		sayHello() {
			console.log(`Hi ${this.name}`)
		}
	};
	return Person;
}());

const me = new Person('Lee');
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

```

## 19.9.2 인스턴스에 의한 프로토타입 교체

```js
function Person(name) {
	this.name = name;
}
const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
	sayHello() {
		console.log(`Hi ${this.name}`)
	}
};

// (1) me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래와 동일하게 동작
// me.__proto__ = parent;

me.sayHello(); // Hi Lee
```

생성자 함수에 의한 프로토타입 교체와 인스턴스에 의한 프로토타입 교체는
다음과 같은 차이가 있다.

- 생성자 함수에 의한 프로토타입 교체
    - Person 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
- 인스턴스에 의한 프로토타입 교체
    - Person 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리키지 않는다.

또한 마찬가지로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되는데 아래와 같이 되살릴 수 있다.

```js
function Person(name) {
	this.name = name;
}
const me = new Person('Lee');

// 프로토타입으로 교체할 객체
const parent = {
	// constructor 프로퍼티와 생성자 함수 간의 연결을 설정
	construcotr: Person,
	sayHello() {
		console.log(`Hi ${this.name}`)
	}
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);

me.sayHello(); // Hi Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

이처럼 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 번거롭다.
프로토타입은 직접 교체하지 않는 것이 좋다.

직접 상속이 편리하며 더욱 안전하다.

# 19.10 instanceof 연산자

instnaceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자
우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다.
우변의 피연산자가 함수가 아닌 경우 TypeError 가 발생한다.

```js
객체 instanceof 생성자함수
```

**우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고 아니면 false**

프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라
**생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.**

```js
const Person = (function () {
	function Person(name) {
		this.name = name;
	}
	// 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
	Person.prototype = {
		sayHello() {
			{...}
		}
	}
	return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person); // false

console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```

# 직접 상속
## 19.11.1 Object.create 에 의한 직접 상속
다른 객체 생성과 마찬가지로 추상 연산 OrdinaryObjectCreate를 호출한다.

이 메서드의 장점은 다음과 같다
- new 연산자가 없어도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

```js
// 프로토타입이 null인 객체를 생성 (생성 된 객체는 체인의 종점에 위치한다.)
// obj -> null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못함
console.log(obj.toString());

// obj -> Object.prototype -> null
// obj = {}; 동일
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj -> Object.prototype -> null
// obj = { x: 1 }; 과 동일
obj = Object.create(Object.prototype, {
	x: {value: 1, writable: true, enumerable: true, configurable: true}
});
// 위 코드는 아래와 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj -> myProto -> Object.prototype -> null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
	this.name = name;
}
// obj -> Person.prototype -> Object.prototype -> null
// obj = new Person('Lee') 와 동일
obj = Object.create(Person.prototype);
obj.name = 'Lee';
console.log(obj.name);
console.log(Object.getPrototype(obj) === Person.prototype); // true
```

Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문에
ESLint에서는 Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다.
**프로토타입 체인의 종점에 위치하는 객체는 Object.prototype 빌트인 메서드를 사용할 수 없다.**
때문에 이 경우 간접 호출을 활용한다.

## 19.11.2 객체 리터럴 내부에서 \_\_proto\_\_  에 의한 직접 상속
Object.create 메서드에 의한 직접 상속은 여러 장점이 있다.
하지만 두 번째 인자로 프로퍼티를 정의하는 것이 번거롭다.
객체를 생성한 이후 추가하는 방법도 있으나 깔금하지 않다.

ES6에서는 객체 리터럴 내부에서 \_\_proto\_\_  접근자 프로퍼티를 사용하여 직접 상속을 구현한다.
```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
	y: 20,
	// 객체를 직접 상속 받는다.
	// obj -> myProto -> Object.prototype -> null
	__proto__: myProto
}

// 위와 동일하다.
const obj2 = Object.create(myProto, {
	y: {value: 20, writable: true, enumerable: true, configruable: true}
})

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```
# 19.12 정적 프로퍼티/메서드
정적 프로퍼티 / 메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.

```js
// 생성자 함수
function Person(name) {
	this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
	console.log(`Hi ${this.name}`);
}

// 증적 프로퍼티
Person.staticProp = 'static prop';
// 정적 메서드
Person.staticMethod = function () {
	console.log('staticMethod');
}

const me = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 있다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드 라고 한다.
생성자 함수가 생성한 인스턴스로 정적 프로퍼티/메서드를 참조/호출할 수 없다.

# 프로퍼티 존재 확인

## 19.13.1 in 연산자
```js
// key in object
const person = {
	name: 'Lee',
	address: 'Seoul'
}

console.log('name' in person); // true
console.log('address' in person); // true
console.log('age' in person); // false

// [!] in 연산자는 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인한다.
console.log('toString' in person); // true

```

in 연산자 대신 ES6에 도입된 Reflect.has 메서드 를 사용할 수도 있다.
Reflect.has 메서드는 in 연산자와 동일하게 동작한다.

```js
const person = { name: 'Lee' };
console.log(Reflect.has(person, 'name')); // true
console.log(Reflect.has(person, 'toString')); // true
```

## 19.13.2 Object.prototype.hasOwnProperty 메서드
Object.prototype.hasOwnProperty 메서드는 인수로 전달 받은 프로퍼티 키가 객체의 고유한 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.
```js
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('age')); // false
console.log(person.hasOwnProperty('toString')); // false
```

# 19.14 프로퍼티 열거
## 19.14.1 for...in 문

객체의 모든 프로퍼티를 순회하며 열거 하려면 for ... in 문을 사용한다.
```js
const person = {
	name: 'Lee',
	address: 'Seoul'
};

for(const key in person) {
	console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

**for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 \[\[Enumerable]] 의 값이 true인 프로퍼티를 순회하며 열거한다.**

또한 심벌인 프로퍼티는 열거하지 않는다.

## 19.14.2 Object.keys / values / entries 메서드
객체 자신의 고유 프로퍼티만 열거하기 위해서는 for...in 문을 사용하는 것보다
Object.keys / values / entries 메서드를 사용하는 것을 권장한다.

Object.keys : 열거 가능한 프로퍼티 키를 배열로 반환
Object.values : 객체 자신의 열거 가능한 프로티 값을 배열로 반환
Object.entries (ES6) : 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

# 알고 가기
- prototype 과 \_\_proto\_\_ 의 차이점에 대해 설명해주세요.

- 프로토타입 체인에 대해 설명해주세요.

- 생성자 함수에 대해 설명해주세요.

- 오버로딩과 오버라이딩에 대해 설명해주세요.

- 다음 코드의 출력을 예측하고 왜 그러한지 설명해주세요.

```js
const person = {
	name: 'Lee',
	address: 'Seoul'
};

console.log('name' in person); // (1)
console.log('toString' in person); // (2)
console.log(person.hasOwnProperty('name')); // (3)
console.log(person.hasOwnProperty('toString')); // (4)
```
