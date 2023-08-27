# 17. 생성자 함수에 의한 객체 생성

## 17.1 Object 생성자 함수

> new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후
> 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
	console.log('Hi! My Name is' + this.name)
}

console.log(person); // { name: "Lee", sayHello: f }
perosn.sayHello(); // Hi My name is Lee
```

> 생성자 함수란 new 연산자와 함께 호출하여 객체를 생성하는 함수를 말한다.
> 생성자 함수에 의해 생성된 객체를 인스턴스라 한다.
> Object 생성자 함수 이외에도 String , Number, Boolean, Function, Array, Date, RegExp Promise 등의 빌트인 생성자 함수를 제공한다.

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

> 객체 리터럴에 의한 객체 생성 방식은 직관적이고 간편하다. 하지만 객체 리터럴에 의한 객체 생성 방식은
> 단 하나의 객체만 생성한다. 동일한 프로퍼티를 갖는 객체를 여러 개 생성하는 경우 매번 같은 프로퍼티를 기술해야하기 때문에
> 비효율적이다.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

> 생성자 함수에 의한 객체 생성 방식은 마치 객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```js
// 생성자 함수
function Circle(radius) {
	// 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

> 생성자 함수는 이름 그대로 객체를 생성하는 함수다. 자바와 같은 클래스 기반 객체지향 언어의 생성자와는 다르게
> 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.


```js
// new 연산자와 함께 호출하지 않으면 생성자 함수는 동작하지 않는다.
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정

> 생성자 함수의역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿 으로서 동작하여
> 인스턴스를 생성하는 것과 생성된 인스턴스를 초기화하는 것이다.

> 생성자 함수가 인스턴스를 생성하는 것은 필수이고, 생선된 인스턴스를 초기화 하는 것은 옵션이다.

```js
// 생성자 함수
function Circle(radius) {
	//인스턴스 초기화
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}

// 인스턴스 생성
const circle1 = new Circle(5);
```

> 생성자 함수 내부의 코드를 살펴보면 this 에 프로퍼티를 추가하고 필요에 따라 전달된 인수를 프로퍼티 초기값으로서 할당하여
> 인스턴스를 초기화한다. 하지만 인스턴스를 생성하고 반환하는 코드는 보이지 않는다.

> 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다. new 연산자와 함께 생성자 함수를 호출하면
> 자바스크립트 엔진은 다음과 같은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환한다.

### 1. 인스턴스 생성과 this 바인딩

> 암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스다.
> 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this 에 바인딩된다. 

### 2. 인스턴스 초기화

> 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 this 에 바인딩되어 있는 인스턴스를 초기화한다.

```js
function Circle(radius) {
	// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

	// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}
```

### 3. 인스턴스 반환

> 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.

```javascript
function Circle(radius) {
    // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle { radius:1, getDiameter: f }
```

> 만약 this 가 아닌 다른 객체를 명시적으로 반환하면 this 가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

```javascript
function Circle(radius) {
	// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
	
	// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
	// 명시적으로 객체를 반환하면 this 반환이 무시된다.
	return {}
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
```

> 하지만 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this 가 반환된다.

```javascript
function Circle(radius) {
	// 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
	
	// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
	// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
	// 명시적으로 원시 값을 반환 하면 원시값 반환은 무시되고 this가 반환된다.
	return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle { radius:1, getDiameter: f }
```

### 17.2.4 내부 메서드

> 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.

> 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
> 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론 함수로서 동작하기 위해 함수 객체만을 위한
> [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고있다.

> 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]] 가 호출된다.

> 함수 객체는 constructor 일 수도있고 non-constructor 일 수도 있다.

### 17.2.5 constructor 와 non-constructor 구분

> 자바스크립트 엔진이 어떻게 구분되는지 살펴보자 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라
> 함수를 constructor 와 non-constructor 로 구분한다.

```js
// 일반 함수 정의: 함수 선언문, 식
function foo() {}
const bar = function () {}

// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않음.
const baz = {
	x: function () {}
};

// 일반 함수로 정의된 함수만이 constructor다.
new foo(); // foo {}
new bar(); // bar {}

new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const obj = {
	x() {}
}

new obj.x(); // TypeError: obj.x is not a construtor
```

### 17.2.6 new 연산자

> 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다.
> new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다.
> 다시 말해 함수 객체의 내부 메서드 Call 이 호출되는 것이 아니라 Construct 가 호출된다.
> new 연산자와 함께 호출하는 함수는 constructor 이어야한다.

```javascript
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
	return x + y;
}
// 생성자 함수로서 정의하지 않은 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
	return { name, role }
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');
// 함수가 생성한 객체를 반환한다.
console.log(inst); // { name: "Lee", role: "admin" }
```

> 반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다. 다시 말해 함수 객체의 내부 메서드 [[Construct]] 가 호출되는 것이 아니라 [[Call]]이 호출된다.

```js
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

### 17.2.7 new.target

> 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해서 파스칼 케이스 컨벤션을 사용한다 하더라도
> 실수는 언제나 발생할 수 있다. 이러한 위험성을 회피하기 위해 ES6 에서는 new.target 을 지원한다.

> new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target 은 함수 자신을 가리킨다.
> new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target 은 undefined 다.

```js
// 생성자 함수
function Circle(radius) {
	// 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
	if(!new.target) {
		// new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
		return new Circle(radius);
	}
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```

> String Number Boolean 생성자 함수는 new 연산자와 함께 호출했을 때
> String Number Boolean 객체를 생성하여 반환합니다. 


---

생성자 함수를 사용했을 경우의 장단점
