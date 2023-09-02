클로저는 자바스크립트 고유의 개념이 아니다.
함수를 일급 객체로 취급하는 함수형 프로그래밍 언어(파스켈, 리스프, 얼랭, 스칼라 등)에서 사용되는 중요한 특성이다.

클로저는 자바스크립트 고유 개념이 아니기 때문에 ECMAScript 사양에 등장하지 않는다.
MDN에서는 클로저를 아래와 같이 정의하고 있다.

>클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

```js
const x = 1;
function outerFunc() {
	const x = 10;
	function innerFunc() {
		console.log(x); // 10
	}
	innerFunc();
}
outerFunc();
```
이와 같이 중첩 함수 innerFunc에서 x 변수에 접근할 수 있는데 이 같은 현상이 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.

# 24.1 렉시컬 스코프
#렉시컬스코프 #정적스코프
**자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 이를 렉시컬 스코프(정적스코프)라 한다.**

"함수의 상위 스코프를 결정한다" = "렉시컬 환경의 **외부 렉시컬 환경에 대한 참조**에 저장할 참조값을 저장한다."

이는 렉시컬 환경의 "**외부 렉시컬 환경에 대한 참조**"에 저장할 참조값이 바로 상위 렉시컬 환경에 대한 참조이며,
이것이 **상위 스코프**이기 때문이다.

정리하자면 렉시컬 스코프는 다음과 같다.
**렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장할 참조값,
즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.**


# 24.2 함수 객체의 내부 슬롯 \[\[Environment\]\]
함수가 정의된 환경(위치)과 호출되는 환경(위치)은 다를 수 있다.
따라서 렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과는 상관없이 자신이 정의된 환경,
즉 상위 스코프(함수 정의가 위치하는 스코프가 바로 상위스코프다)를 기억해야한다.

함수는 자신의 내부 슬롯 \[\[Environment\]\]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

함수 정의가 평가되어 함수 객체를 생성할 때 위치에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯 \[\[Environment\]\] 에 저장한다.
이때 \[\[Environment\]\] 에 저장된 상위 스코프의 참조는 현재 실행중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.

함수 내부에서 정의된 함수 표현식은 외부 함수 코드가 실행되는 시점에 평가되어 함수 객체를 생성한다.
이때 생성된 함수 객체의 내부 슬롯 \[\[Environment\]\] 에는 함수가 정의가 평가되는 시점
즉 외부 함수 코드 실행 시점에 실행중인 실행 컨텍스트의 렉시컬 환경인 외부 함수 렉시컬 환경에 참조가 저장된다.

정리하자면,
**함수 객체의 내부 슬롯 \[\[Environment\]\]에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위스코프다.
또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조"에 저장될 참조값이다.
함수 객체는 내부 슬롯 \[\[Environment\]\]에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억해야 한다.**

```js
const x = 1;

function foo() {
	const x = 10;
	// 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
	// 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
	bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
	console.log(x);
}

foo(); // 1
bar(); // 1
```

foo 함수와 bar 함수는 모두 전역 코드가 평가되는 시점에 평가되어 전역 객체 window의 메서드가 된다.
생성된 함수 객체의 내부슬롯 \[\[Environment\]\]에는 평가된 시점 즉 전역 렉시컬 환경의 참조가 저장된다.

이후 함수가 호출되면 아래와 같이 함수 평가가 이루어진다.
1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
    1. 함수 환경 레코드 생성
    2. this 바인딩
    3. 외부 렉시컬 환경에 대한 참조 결정

외부 렉시컬 환경에 대한 참조에는 함수 객체의 내부 슬롯 \[\[Environment\]\]에 저장된 렉시컬 환경의 참조가 할당된다.

# 24.3 클로저와 렉시컬 환경

```js
const x = 1;

// (1)
function outer() {
	const x = 10;
	const inner = function () {console.log(x);}; // (2) (함수표현식 : 런타임에 평가)
	return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // (3)
innerFunc(); // 10
```

outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다.
이때 outer 함수의 지역변수 x 와 값 10을 저장하고 있던 outer 함수에 실행 컨텍스트가 제거되었으므로 제거되었으므로 outer 함수의 지역 변수 x 또한 생명 주기를 마감한다.

하지만 결과 값은 10이다.
이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저라고 부른다.**

**outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되었지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.**
outer 함수의 렉시컬 환경은 inner 함수의 \[\[Environment\]\] 내부 슬롯에 의해 참조되고 있고 inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다.
하지만 일반적으로 모든 함수를 클로저라고 하지 않는다.

### **상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다.**
상위 스코프의 어떤 식별자도 참조하지 않으면 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다.
```js
function foo() {
	const x = 1;
	const y = 2;
	
	// bar함수는 클로저가 아니다.
	function bar() {
		const z = 3;
		console.log(z);
	}
	return bar;
}
const bar = foo();
bar();
```

아래 예제의 bar는 상위 스코프의 식별자를 참조하고 있으므로 클로저다.
하지만 외부 함수 foo의 외부로 중첩함수 bar가 반환되지 않는다.
즉 외부함수 foo 보다 중첩함수 bar의 생명주기가 짧다.

```js
function foo() {
	const x = 1;
	function bar() {
		cnosole.log(x);
	}
	bar();
}
foo();
```

중첩함수 bar는 클로저 였지만 외부 함수보다 일찍 소멸되기 때문에 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다.
따라서 일반적으로 클로저라고 하지않는다.

**클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.**

```js
function foo() {
	const x = 1;
	const y = 2;

	// 클로저
	function bar() {
		console.log(x);
	}
	return bar;
}

const bar = foo();
bar();
```
bar는 상위 스코프 x, y 식별자 중 x 만 참조하고 있다.
이런 경우 대부분의 모던 브라우저는 최적화를 통해 참조하고 있는 식별자만을 기억한다.
클로저에 의해 참조되는 상위 스코프 변수를 **자유 변수**라고 부른다. ( x는 자유 변수다 )

# 24.4 클로저의 활용
**클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.**
상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉** 하고 **특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

```js
let num = 0;

const increase = function() {
	return ++num;
}

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 예제는 전역 변수 num 의 값이 변경되는 이는 오류로 이어진다.

```js
const increase = function() {
	let num = 0;
	
	return ++num;
}

console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

이제 변수 num의 상태는 increase 함수만이 변경할 수 있다.
num 변수는 increase 함수가 호출될 때마다 0으로 초기화되어 항상 1이다.

```js
// 클로저
const increase = (function() {
	let num = 0;
	return function () {
		return ++num;
	}
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

즉시 실행 함수가 호출되고 즉시 실행함수가 반환한 함수가 increase 변수에 할당된다.
increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.

따라서 즉시 실행 함수가 반환한 클로저는 자유변수 num을 언제 어디서든 호출하든지 참조하고 변경할 수 있다.
또한 private 변수이므로 외부에서 접근할 수 없다.

이처럼 **클로저는 상태가 의도치 않게 변경되지 않도록 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

개선하면 다음과 같이 사용할 수 있다.
```js
const counter = (function () {
	let num = 0;

	// 클로저인 메서드를 갖는 객체를 반환한다.
	// 객체 리터럴은 스코프를 만들지 않는다.
	// 따라서 아래 메서드들의 스코프는 즉시 실행 함수의 렉시컬 환경이다.
	return {
		// num: 0, // 프로퍼티는 public 하므로 은닉되지 않음.
		increase() {
			return ++num;
		},
		decrease() {
			return --num;
		}
	}
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
```

위 코드를 생성자로 표현하면 다음과 같다.

```js
const Counter = (function {
	let num = 0;
	
	function Counter() {
		// this.name = 0; // 프로퍼티는 public하므로 은닉되지 않음
	}

	Counter.prototype.increase = function () {
		return ++num;
	};
	
	Counter.prototype.decrease = function () {
		return --num;
	};
	
	return Counter;
}());

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
```

increase, decrease 메서드 모두 자신의 함수 정의가 평가되어 함수 객체가 될때 실행중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환겨을 기억하는 클로저다.

또 다른 간단한 예제
```js
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
	let counter = 0;

	// 클로저를 반환
	return function() {
		counter = aux(counter);
		return counter;
	}
}

// 보조 함수
function increase(n) {
	return ++n;
}
function decrease(n) {
	return --n;
}

const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동되지 않음.
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

주의할 점은 **makeCounter 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다는 것**이다.

아래와 같이 독립적인 카운터가 아니라 자유변수를 공유하는 클로저를 만들 수 있다.

```js
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유변수 counter를 기억하는 클로저를 반환한다.
const counter = (function() {
	let counter = 0;
	return function(aux) {
		// 인수로 전달받은 보조 함수에 상태 변화를 위임
		counter = aux(counter);
		return counter;
	}
})

// 보조 함수
function increase(n) {
	return ++n;
}
function decrease(n) {
	return --n;
}

console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(increase)); // 0

```

# 24.5 캡슐화와 정보 은닉
캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

정보 은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부로부터 감추어 적적하지 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호한다.
또한 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.

자바스크립트의 모든 프로퍼티느와 메서드는 기본적으로 public 이다.

```js
function Person(name, age) {
	this.name = name; // public
	let _age = age; // private
	// 인스턴스 메서드
	this.sayHi = function () {
		console.log(`name ${this.name} age: ${_age}`);
	};
}

const me = new Person('Lee',20);
me.sayHi(); // name Lee age: 20
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim',30);
you.sayHi(); // name Kim age: 30
console.log(you.name); // Kim
console.log(you._age); // undefined
```

인스턴스 메서드를 프로토타입으로 변경하여 중복 생성을 방지해보자.

```js
function Person(name, age) {
	this.name = name;
	let _age = age;
}

Person.prototype.sayHi = function () {
	// Person생성자 함수의 지역 변수 _age 를 참조할 수 없다.
	console.log(`name ${this.name} age: ${_age}`); // ERROR
}
```

해결하기 위해 즉시 실행 함수로 사용하여 보자.

```js
const Person = (function() {
	let _age = 0; // private
	// 생성자 함수
	function Person(name, age) {
		this.name = name;
		_age = age;
	}
	// 프로토타입 메서드
	Person.prototype.sayHi = function () {
		console.log(`name ${this.name} age: ${_age}`);
	}
	// 생성자 함수 반환
	return Person;
}());

const me = new Person('Lee',20);
me.sayHi(); // name Lee age: 20
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim',30);
you.sayHi(); // name Kim age: 30
console.log(you.name); // Kim
console.log(you._age); // undefined
```

위 패턴을 통해 정보 은닉이 가능한 것 처럼 보인다.

하지만 문제점이 있다. `\_age` 변수의 상태가 유지되지 않는다는 것이다.

```js
const me = new Person('Lee',20);
me.sayHi(); // name Lee age: 20

const you = new Person('Kim',30);
you.sayHi(); // name Kim age: 30

me.sayHi(); // name Lee age: 30
```

이는 Person.prototype.sayHi 메서드가 단 한번 생성되는 클로저이기 때문이다.

Person.prototype.sayHi 메서드는 즉시 실행 함수가 호출될 때 생성된다.
Person.prototype.sayHi 메서드는 자신의 상위 스코프인 즉시실행 함수의 \[\[Environment\]\] 에 저장하여 기억한다.
따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 상위 스코프를 공유하게 된다.

# 24.6 자주 발생하는 실수

```js
var funcs = [];
for(var i = 0; i < 3; i++) {
	funcs[i] = function () { return i; } // ?
}
for(var j = 0; i < 3; i++) {
	console.log(funcs[j]()); // ?
}
```

0,1,2 를 기대하지만 3을 출력한다.
var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수이기 때문이다.
i는 전역 변수를 참조하여 3이 출력한다.

클로저를 통해 이 방법을 해결할 수 있으나 ES6 let 키워드를 통해 번거로움이 사라졌다.

```js
var funcs = [];
for(let i = 0; i < 3; i++) {
	funcs[i] = function () { return i; } // ?
}
for(let j = 0; i < 3; i++) {
	console.log(funcs[j]()); // 0 1 2
}
```

for문의 변수 선언문에서 let 키워드로 선언한 변수를 사용하면 for문의 코드 블록이 반복 실행될 때마다 for문 코드 블록의 새로운 렉시컬 환경이 생성된다.

만약 for문의 코드 블록 내에서 정의한 함수가 있다면 이 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행될 때마다 생성된 for문 코드 블록의 새로운 렉시컬 환경이다.
(그림 24-11 | 415p 참고)

이처럼 let 이나 const 키워드를 사용하는 반복문은 코드 블록을 반복 실행할 때마다
새로운 렉시컬 환경을 생성하여 반복할 당시의 상태를 스냅숏을 찍는 것처럼 저장한다.

또 다른 방법으로 고차 함수를 사용하는 방법이 있다.

```js
// 배열의 요소로 추가된 함수들은 모두 클로저다
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [f, f, f]

funcs.forEach(f => console.log(f())); // 0 1 2
```

# 알고 가기
---
- [ ] 클로저란 무엇인가요?
