# 25.1 클래스는  프로토타입의 문법적 설탕인가?
프로토타입 기반 객체지향 언어는 클래스가 필요 없는 객체지향 프로그래밍 언어다.

ES5 에서는 클래스 없이도 다음과 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.
```js
// ES5 생성자 함수
var Person = (function (){
	function Person(name) {
		this.name = name;
	}
	Person.prototype.sayHi = function () {
		console.log(`Hi ${this.name}`);
	};
	return Person;
}());

var me = new Person('Lee');
me.sayHi(); // Hi Lee
```

클래스 기반 언어에 익숙한 프로그래머들은 프로토타입 기반 프로그래밍 방식에 혼란을 느낄 수 있다.

ES6의 클래스가 기존의 프로토타입 기반 객체지향 모델을 폐지하고 새롭게 클래스 기반 객체지향 모델을 제공하는 것은 아니다.

사실 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕이라고 볼 수도 있다.

**클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다.**

다음과 같은 차이점이 있다.
1. 클래스를 new 연사자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수를 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. 클래스는 상속을 지원하는 extends 와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 \[\[Enumerable]]]의 값이 false다. 열거되지 않는다.

클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료하다.
특히 클래스의 extends 와 super 키워드는 상속 관계 구현을 더욱 간결하고 명료하게 한다.

따라서 클래스를 문법적 설탕으로 보기보다는 **새로운 객체 생성 메커니즘** 으로 보는 것이 좀 더 합당하다.

# 25.2 클래스 정의

클래스는 다음과 같이 선언할 수 있다.
```js
// 클래스 선언문
class Person {}

// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스가 표현식으로 정의할 수 있다는 것은 클래스가 값으로 사용할 수 있는 **일급 객체**라는 것을 의미한다.
클래스는 일급객체로서 다음과 같은 특징을 갖는다.
- 무명의 리터럴로 생성할 수 있다. 즉 런타임에 생성이 가능하다.
- 변수나 자료구조(객체, 배열, 등)에 저장할 수 있다.
- 함수의 매개변수에게 전달할 수 있다.
- 함수의 반환값으로 사용할 수 있다.

자세히 말하자면 클래스는 함수다. 값으로 사용할 수 있는 일급객체다.

```js
// 클래스 선언문
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name; // public property
	}
	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi ${this.name}`);
	}
	// 정적 메서드
	static sayHello() {
		console.log('Hello');
	}
}

const me = new Person('Lee');
console.log(me.name); // Lee
me.sayHi(); // Hi Lee
Person.sayHello(); // Hello
```

# 25.3 클래스 호이스팅
클래스는 함수로 평가된다.
```js
// 클래스 선언문
class Person {}
console.log(typeof Person); // function
```
클래스 선언문으로 정의한 클래스는 함수선언문과 같이 소스코드 평가 과정
즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.

단 클래스는 클래스 정의 이전에 참조할 수 없다.
```js
console.log(Person); // ReferenceError: Cannot access 'Person' before initialization

class Person {}
```

클래스 선언문은 호이스팅이 발생한다.

```js
const Person = '';
{
	// 호이스팅이 발생하지 않는다면 '' 가 출력되어야 한다.
	console.log(Person);
	// ReferenceError: Cannot access 'Person' before initialization
	
	// 클래스 선언문
	class Person {}
}
```
클래스 선언문도 변수 선언 함수 정의와 같이 호이스팅이 발생한다.
하지만 TDZ에 의하여 호이스팅이 발생하지 않는 것 처럼 동작한다.

var, let, const, function, function*, class 를 사용하여 선언된 모든 식별자는 호이스팅 된다.
# 25.4 인스턴스 생성
```js
const Person {}
// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}
```

new 연산자 여부에 따라 일반 함수로 호출되거나 생성자 함수로 호출되지만
클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출하자.

```js
const Person {}
// new 키워드 생략하면 타입 에러가 발생한다.
const me = Person(); // TypeError: Class constructor Foo cannot be invoked wihout 'new'
```

기명 클래스 표현식의 클래스 이름(MyClass)을 사용해 인스턴스를 생성하면 에러가 발생한다.
```js
// 기명 클래스 표현식
const Person = class MyClass {};

const me = new Person();

// 클래스 이름 MyClass 는 함수와 동일하게 클래스를 통해 몸체 내부에서만 유효한 식별자이다.
console.log(MyClass); // ReferenceError: MyClass is not defined

const you = new MyClass(); // ReferenceError: MyClass is not defined
```

# 25.5 메서드
클래스 몸체에는 0개 이상의 메서드만 선언할 수 잇다.

## 25.5.1 constructor
```js
class Person {
	// 생성자
	constructor(name) {
		// 인스턴스 생성 및 초기화
		this.name = name;
	}
}
```

```js
// 클래스는 함수다.
console.log(typeof Person); // function
console.dir(Person);
```

콘솔을 브라우저에 실행해보자.
살펴보면 클래스는 평가되어 함수 객체가 된다.

클래스도 함수 객체 고유의 프로퍼티를 모두 갖고 있다.
함수와 동일하게 프로토타입과 연결되어 있으며 자신의 스코프체인을 구성한다.

모든 함수가 갖고있는 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리키고 있다. 이는 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미한다.
즉 new 연산자와 함께 클래스를 호출하면 클래스는 인스턴스를 생성한다.

```js
const me = new Person('Lee');
console.log(me);
```

마찬가지로 브라우저에 실행해보자.

constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 된다.
constructor 내부의 this 는 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스를 가리킨다.

흥미로운점은 클래스가 평가되어 생성도니 함수 객체나 클래스가 생성한 인스턴스 어디에도 constructor 메서드가 보이지 않는다는 것이다.
이는 클래스 몸체에 정의한 constructor는 단순한 메서드가 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.
정리하자면 클래스가 정의되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

### 클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티
> 클래스의 constructor 메서드와 프로토타입 constructor 프로퍼티는 이름이 같아 혼동하기 쉽지만
> 직접적인 관련이 없다.
> 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티이며 생성자 함수를 기리킨다.

constructor는 클래스 내에 최대 한 개만 존재할 수 있다.
```js
class Person {
	constructor() {}
	constructor() {}
}
// SyntaxError : ...
```

constructor는 생략할 수 있다.
생략하면 빈 constructor가 암묵적으로 정의된다.

```js
class Person {}

// 위와 같다.
class Person {
	constructor() {}
}
```

프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.

```js
class Person {
	constructor(name) {
		this.name = name; // 인수로 인스턴스 초기화
		this.address = 'Seoul'; // 고정값으로 인스턴스 초기화
	}
}
const me = new Person();
console.log(me);
```

constructor 에 별도의 반환문을 갖지 않아야 한다.
암묵적으로 this, 즉 인스턴스를 반환하기 때문이다.

다른 객체를 명시적으로 반환하면 this, 즉 인스턴스가 반환되지 못하고 return 문에 명시한 객체가 반환된다.

```js
class Person {
	constructor(name) {
		this.name = name;
		return {};
	}
}
const me = new Person('Lee');
console.log(me); // {}
```

하지만 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.

```js
class Person {
	constructor(name) {
		this.name = name;
		return 100;
	}
}
const me = new Person('Lee');
console.log(me); // Person { name: 'Lee' }
```

## 25.5.2 프로토타입 메서드
클래스 몸체에서 정의한 메서드는 생성자 함수에 의한 객체 생성 방식과는 다르게
클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```js
class Person {
	constructor(name) {
		this.name = name;
	}
	// 프로토타입 메서드
	sayHi() {
		console.log(`Hi ${this.name}`);
	}
}
```

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

```js
// me 객체와 프로토타입은 Person.prototype 이다.
Object.getPrototypeOf(me) === Person.prototype; // true
me instanceof Person; // true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
me instanceof Object; // true

// mew 객체의 constructor는 Person 클래스이다.
me.constructor === Person; // true
```

이처럼 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 된다.

프로토타입체인은 기존의 모든 객체 생성방식 뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다. 생성자 함수의 역할을 클래스가 할 뿐이다.

## 25.5.3 정적 메서드
인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.

```js
// 생성자 함수
function Person(name) {
	this.name = name;
}
Person.sayHi = function () {
	console.log('Hi')
}

// class
class Person {
	constructor(name) {
		this.name = name;
	}
	static sayHi() {
		console.log('Hi');
	}
}
```

클래스는 함수 객체로 평가되므로 자신의 프로퍼티/메서드를 소유할 수 있다.
이처럼 정적 메서드는 클래스에 바인딩된 메서드가 된다.

정적 메서드는 프로토타입 메서드처럼 인스턴스로 호출하지 않고 클래스로 호출한다.

## 25.5.4 정적 메서드와 프로토타입 메서드의 차이
1. 정적 메서드와 프로토타입 메서드는 자신이 속해있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

```js
class Square1 {
	// 정적 메서드
	static area(w, h) {
		return w * h;
	}
}
console.log(Square1.area(10, 10)) // 100
class Square2 {
	constructor(width, height) {
		this.w = width;
		this.h = height;
	}
	// 프로토타입 메서드
	area() {
		return this.w * this.h;
	}
}
const square = new Square2(10, 10);
console.log(square.area(10, 10)) // 100
```

프로토타입 메서드 내부의 this는 프로토타입 메서드를 호출한 인스턴스를 가리킨다.
정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킨다.

this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다.

## 25.5.5 클래스에서 정의한 메서드의 특징
1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. for...in 문이나 Object.keys 메서드 등으로 열거할 수 없다. \[\[Enumerable]] 이 false다.
5. 내부 매서드 \[\[Construct]]를 갖지 않는 non-constructor다. new 연산자와 함께 호출할 수 없다.

# 25.6 클래스 인스턴스 생성 과정
new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래수의 내부 메서드 \[\[Construct]]가 호출된다.
다음과 같은 과정을 거쳐 인스턴스가 생성된다.
### 1. 인스턴스 생성과 this 바인딩
new 연산자와 함께 클래스를 호출하면 constructor 내부 코드가 실행되기 앞서 암묵적으로 빈 객체가 생성됨.
빈 객체는 클래스가 생성한 인스턴스다.
클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다.
암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.
따라서 constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킨다.
### 2. 인스턴스 초기화
constructor의 내부 코드가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다.
constructor가 생략되어있다면 이 과정도 생략된다.
### 3. 인스턴스 반환
클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
```js
class Person {
	constructor(name) {
		// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
		console.log(this); // Person {}
		console.log(Object.getPrototype(this) === Person.prototype); // true

		// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
		this.name = name;	

		// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
	}
}
```

# 25.7 프로퍼티
## 25.7.1 인스턴스 프로퍼티
인스턴스 프로퍼티는 constructor 내부에 정의한다.

constructor 내부 코드가 실행되기 이전에 constructor 내부의 this에는 이미 클래스가 암묵적으로 생성한 인스턴스인 빈 객체가 바인딩되어 있다.
constructor 내부에서 this에 인스턴스 프로퍼티를 추가한다.
이로써 암묵적으로 생성한 빈 객체, 즉 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화 된다.

```js
class Person {
	constructor(name) {
		// 인스턴스 프로퍼티
		this.name = name;
	}
}
```

## 25.7.2 접근자 프로퍼티
접근자 프로퍼티는 클래스에서도 사용할 수 있다.
```js
class Person {
	constructor(firstName, lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}
	get fullName() {
		return `${this.firstName} ${this.lastName}`
	}
	set fullName() {
		[this.firstName, this.lastName] = name.split(' ');
	}
}

// fullName은 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
// {get:f, set:f, enumerable: false, configurable: true}
```

## 25.7.3 클래스 필드 정의 제안
클래스 필드(필드 또는멤버)는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.

자바스크립트의 클레스 몸체에는 메서드만 선언할 수 있다.
따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생한다.
하지만 최신 브라우저(Chrome 72이상) 또는 최신 Node.js(v12이상) 에서 실행하면 문법 에러가 발생하지 않고 정상 동작한다.

```js
class Person {
	name = 'Lee';
}
const me = new Person('Lee');
```

그 이유에 대해 알아보자.

자바스크립트에도 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양인 "Class field declarations"가 2021.01 현재 TC39 프로세스의 stage3(candidate)에 제안되어 있다.

클래스 몸체에 클래스 필드를 정의할 수 있는 클래스 필드 정의 제안은 표준 사양으로 승급되지 않았다.
하지만 최신 브라우저와 최신 Node.js 버전에서는 사용할 수 있도록 미리 구현해 놓았다.

```js
class Person {
	name = 'Lee';
}
const me = new Person('Lee');
console.log(me); // Person {name: 'Lee'}
```

예제를 보면서 클래스 필드 정의에 대해 알아보자.

```js
class Person {
	// this에 클래스 필드를 바인딩해서는 안된다.
	this.name = '' // SyntaxError
}
```

```js
class Person {
	name = 'Lee';
	constructor() {
		// Java 에서는 생략이 가능하지만 자바스크립트에서는 this 생략이 불가능하다.
		console.log(name); // ReferenceError
	}
}
```

```js
class Person {
	// 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
	name;
}
const me = new Person();
console.log(me); // Person {name: undefined}
```

클래스 필드를 외부 인자를 통해 초기화해야한다면 constructor에서 초기화한다.
```js
class Person {
	name; // 생략가능
	constructor(name) {
		this.name = name;
	}
}
const me = new Person('Lee');
console.log(me); // Person {name: 'Lee'}
```
이처럼 클래스 필드를 초기화할 필요가 있다면 constructor 밖에 정의할 필요가 없다.
클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티가 없다면 자동으로 추가되기 때문이다.

함수를 클래스 필드에 할당할 수도 있다.
```js
class Person {
	// 문자열을 할당
	name = 'Lee'
	// 함수를 할당
	getName = function () { return this.name; }
}
```

클래스 필드에 함수를 할당하는 경우 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.
**모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다.**

따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.

## 25.7.4 private 필드 정의 제안
private (#) 는 2021년 1월 표준사양이 제안되었고 최신 브라우저와 최신 Node.js 에서 이미 구현되어있다.

```js
class Person {
	#name = '';
	constructor(name){
		this.#name = name;
	}

	// name은 접근자 프로퍼티다.
	get name() {
		return this.#name
	}
}
const me = new Person('Lee');
// 클래스 외부에서 참조할 수 없다.
console.log(me.#name); // SyntaxError

// 접근자 프로퍼티를 통해 접근가능하다.
console.log(me.name); // Lee
```

```js
class Person {
	constructor(name) {
		// private 필드는 클래스 몸체에서 정의해야 한다.
		this.#name = name; // SyntaxError: ...
	}
}
```

## 25.7.5 static 필드 정의 제안
static public/private 필드는 최신 브라우저 Chrome72, Node.js12이상 에서 이미 구현되어있다.

```js
class MyMath {
	// static public 필드 정의
	static PI = 22 / 7;
	// static private 필드 정의
	static #num = 10;
	// static method
	static increment() {
		return ++MyMath.#num;
	}
}
console.log(MyMath.PI); // 3.14..
console.log(MyMath.increment()); // 11
```

# 25.8 상속에 의한 클래스 확장
## 25.8.1 클래스 상속과 생성자 함수 상속
클래스 확장은 프로토타입 기반 상속과는 다른 개념이다.
**프로토타입 기반 상속**은 프로토타입 체인을 통해 다른 객체의 자신을 상속받는 개념이지만
**상속에 의한 클래스 확장**은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것이다.

```js
class Animal {
	constructor(age, weight) {
		this.age = age;
		this.weight = weight;
	}
	eat() { return 'eat'; }
	move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
	fly() { return 'fly'; }
}

const bird = new Bird(1, 5);
console.log(brid); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(brid.eat()); // eat
console.log(brid.move()); // move
coonsole.log(brid.fly()); // fly
```

## 25.8.2 extends 키워드
상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

```js
// 슈퍼 (베이스/부모) 클래스
class Base {}
// 서브 (파생/자식) 클래스
class Derived extends Base {}
```

수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다.
이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.

## 25.8.3 동적 상속
extends 키워드는 클래스 뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수도있다.
단 extends 앞에는 반드시 클래스가 와야한다.
```js
function Base(a) {
	this.a = a;
}
// 생성자 함수를 상속받는 서브 클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived {a: 1}
```

extends 키워드 다음에는 클래스뿐만이 아니라 \[\[Construct\]\] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를통해 동적으로 상속받을 대상을 결정할 수 있다.

```js
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

## 25.8.4 서브클래스의 constructor
서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다.
```js
constructor(..args) { super(...args); }
```

## 25.8.5 super 키워드
#super
super 키워드는 함수처럼 호출할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.
super는 다음과 같이 동작한다
- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

### super 호출
super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.
```js
// 수퍼클래스
class Base {
	constructor(a, b) {
		this.a = a;
		this.b = b;
	}
}
class Derived extends Base {
	// 다음과같이 암묵적으로 정의됨.
	// constructor(...args) { super(..args); }
}
const derived = new Derived(1, 2);
console.log(derived); // Derived {a: 1, b: 2}
```

수퍼클래스의 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에서 호출하는 super를 통해 전달한다.
```js
class Derived extends Base {
	constructor(a, b, c) {
		super(a, b)
		this.c = c;
	}
}
const derived = new Derived(1, 2 ,3);
console.log(derived); // Derived {a:1, b:2, c:3};
```

super를 호출할 때 주의사항은 다음과 같다.
1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super를 호출해야한다.
2. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
3. super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.

### super 참조
메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다.
```js
class Base {
	constructor(name) {
		this.name = name;
	}
	sayHi() {
		return `Hi ${this.name}`
	}
}

class Derived extends Base {
	sayHi() {
		return `${super.sayHi()} how are you doing?`
	}
}
const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi Lee how are you doing
```

super 참조를 의사 코드로 표현하면 다음과 같다.

```js
/*
[[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
[[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
*/
super = Object.getPrototypeOf([[HomeObject]]);
```

주의할 점은 ES6의 메서드 축약 표현으로 정의된 함수만이 \[\[HomeObject\]\]를 갖는다는 것이다.
\[\[HomeObject]]를 갖는 함수만이 super를 참조할 수 있다.

super 참조는 클래스의 전유물은 아니다.
객체 리터럴에서도 super 참조를 사용할 수 있다. (단 메서드 축약표현으로 정의된 함수만 가능하다.)

```js
const base = {
	name: 'Lee',
	sayHi() {
		return `Hi ${this.name}`
	}
};

const derived = {
	__proto__ : base,
	sayHi() {
		return `${super.sayHi()} how are you doing?`
	}
}

console.log(derived.sayHi()); // Hi Lee how are you doing?
```

2. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼 클래스의 정적 메서드 sayHi를 가리킨다.
```js
class Base {
	static sayHi() {
		return 'Hi'
	}
}

class Derived extends Base {
	static sayHi() {
		return `${super.sayHi()} how are you doing?`;
	}
}
console.log(Derived.sayHi()); // Hi how are you doing?
```

## 25.8.6 상속 클래스의 인스턴스 생성 과정
상속 관게에 있는 두 클래스가 어떻게 협력하며 인스턴스를 생성하는지 살펴보도록 하자.
```js
class Rectangle {
	constructor(w, h) {
		this.w = w;
		this.h = h;
	}
	getArea() {
		return this.w * this.h;
	}
	toString() {
		return `w = ${this.w} h = ${this.h}`
	}
}

class ColorRectangle extends Rectangle {
	constructor(w, h, c) {
		super(w, h);
		this.c = c;
	}
	// 메서드 오버라이딩
	toString() {
		return super.toString() + `c = ${this.c}`
	}
}


const colorRectangle = new ColorRectangle(2, 4, 'red');

console.log(colorRectangle.getArea()); // 8
console.log(colorRectangle.toString()); // w = 2 h = 4 c = red
```

서브클래스 ColorRectangle이 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스를 생성한다.

1. 서브클래스의 super 호출
   자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기위해 "base" 또는 "derived"를 값으로 갖는 \[\[ConstructorKind\]\]를 갖는다.
   다른 클래스를 상속받지 않는 클래스는 내부슬롯 \[\[ConstructorKind\]\] 값이 "base" 로 설정되고
   다른 클래스를 상속받는 클래스는 내부슬롯 \[\[ConstructorKind\]\] 값이 "derived" 로 설정된다.

이를 통해 new 연산자와 함꼐 호출되었을 때 동작이 구분된다.

다른 클래스를 상속받지 않은 클래스를 new 연산자와 함께 호출되었을 때 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 this를 바인딩한다.

하지만 **서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다.
이것이 바로 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유다.**

2. 수퍼클래스의 인스턴스 생성과 this 바인딩.
   수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다.
   이 빈 객체가 바로 클래스가 생성한 인스턴스다.
   암묵적으로 생성된 빈 객체 즉 인스턴스는 this에 바인딩 된다.
   따라서 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킨다.
```js
class Rectangle {
	constructor(w, h) {
		// 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
		console.log(this);
		// new 연산자와 함께 호출된 함수
		console.log(new.target); // ColorRectangle

		// 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
		console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype)
		// true
		console.log(this instanceof ColorRectangle); // true
		console.log(this instanceof Rectangle); // true
	}
}
```

인스턴스는 수퍼클래스가 생성한 것이다.
하지만 new 연산자와 함께 호출된 클래스가 서브클래스인 것이 중요하다.
즉 new 연산자와 함께 호출된 함수를 가리키는 new.target은  서브클래스를 가리킨다.
따라서 **인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.**

정리하면 생성된 인스턴스의 프로토타입은 수퍼클래스의 prototype 프로퍼티가 가리키는 객체가 아니라 new.target, 즉 서브클래스의 prototype 프로퍼티가 가리키는 객체다.

3. 수퍼클래스의 인스턴스 초기화
   수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
   즉 this에 바인딩 되어있는 인스턴스에 프로퍼티를 추가하고 인수로 전달받은 초기값으로 인스턴스와 프로퍼티를 초기화한다.

```js
class Rectangle {
	constructor(w, h) {
		// 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
		console.log(this);
		// new 연산자와 함께 호출된 함수
		console.log(new.target); // ColorRectangle

		// 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
		console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype)
		// true
		console.log(this instanceof ColorRectangle); // true
		console.log(this instanceof Rectangle); // true

		// 인스턴스 초기화
		this.w = w;
		this.h = h;
		console.log(this); // ColorRectangle {w: 2, h: 4}
	}
}
```

4. 서브클래스 constructor로의 복귀와 this 바인딩
   super의 호출이 종룓되고 제어 흐름이 서브클래스 constructor로 돌아온다.

**super가 반환한 인스턴스가 this에 바인딩된다.
서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.**
```js
class ColorRectangle extends Rectangle {
	constructor(w, h, c) {
		super(w, h);
		// super가 반환한 인스턴스가 this에 바인딩된다.
		console.log(this); // ColorRectangle {w: 2, h: 4}
	}
	...
}
```
**이처럼 super가 호출되지 않으면 인스턴스가 생성되지 않으며 this바인딩도 할 수 없다.
서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 바로 이 때문이다.**

5. 서브클래스의 인스턴스 초기화
   super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다.
   즉 this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

6. 인스턴스 반환
   클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
```js
class ColorRectangle extends Rectangle {
	constructor(w, h, c) {
		super(w, h);
		// super가 반환한 인스턴스가 this에 바인딩된다.
		console.log(this); // ColorRectangle {w: 2, h: 4}
		// 인스턴스 초기화
		this.c = c;]
		// 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
		console.log(this); // ColorRectangle {w: 2, h: 4, c: "red"}
	}
	
	...
}
```

## 25.8.7 표준 빌트인 생성자 함수 확장
```js
class MyArray extends Array {
	// 중복요소를 제거하고 반환
	uniq() {
		return this.filter((v,i,self) => self.indexOf(v) === i);
	}
	average() {
		return this.reduce((pre, cur) => pre + cur, 0) / this.length;
	}
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
console.log(myArray.average()); // 1.75
```

이때 주의할 것은 Array.prototype의 메서드 중 map, filter와 같이 새로운 배열을 반환하는 메서드가 MyArray클래스의 인스턴스를 반환한다는 것이다.
```js
console.log(myArray.filter(v => v % 2) instanceof MyArray); // true
```

만약 MyArray 클래스의 인스턴스를 반환하지 않고 Array의 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝이 불가능하다.

```js
// 메서드 체이닝
// [1, 1, 2, 3] => [1, 1, 3] => [1, 3] = > 2
console.log(myArray.filter(v => v % 2).uniq().average()); // 2
```

myArray.filter 가 반환하는 인스턴스가 MyArray 타입이기 때문에 연이어 메서드 체이닝이 가능한 것이다.

만약 MyArray 클래스가 생성한 인스턴스가 아닌 Array가 생성한 인스턴스를 반환하게 하려면 다음과 같이
Symbol.species를 사용하여 정적 접근자 프로퍼티를 추가한다.
```js
class MyArray extends Array {
	// 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
	static get [Symbol.species]() { return Array; }
	
	uniq() {...}
	average() {...}
}
const myArray = new MyArray(1,1,2,3);
console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.average() instanceof Array); // true
// 메서드 체이닝
// uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
console.log(myArray.uniq().average());
// TypeError: myArray.uniq(...).average is not a function
```

# 알고 가기
---
- [ ] super 에 대해 설명해주세요.
- [ ] 다음 코드를 class 문법으로 바꾸어주세요.
```js
var Person = (function (){
	function Person(name) {
		this.name = name;
	}
	Person.prototype.sayHi = function () {
		console.log(`Hi ${this.name}`);
	};
	return Person;
}());
Person.sayHello = function () {
	console.log('Hello')
}
```

