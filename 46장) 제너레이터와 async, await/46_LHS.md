# 46.1 제너레이터란?

ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

## 제너레이터와 일반함수의 차이점
### 1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다.
즉, 함수 호출자는 함수를 호출한 이후 함수 실행을 제어할 수 없다.

제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다.
이는 **함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다**는 것을 의미한다.

### 2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다. 즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.

제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.
**제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.**

### 3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
**제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.**


# 46.2 제너레이터 함수의 정의

제너레이터 함수는 function* 키워드로 선언한다.
그리고 하나 이상의 yield 표현식을 포함한다.

```js
// 제너레이터 함수 선언문
function* genDecFunc() {
	yield 1;
}
// 제너레이터 함수 표현식
const genExpFunc = function* () {
	yield 1;
}
// 제너레이터 메서드
const obj = {
	* genObjMethod() {
		yield 1;
	}
}
// 제너레이터 클래스 메서드
class MyClass {
	* genClsMethod() {
		yield 1;
	}
}
```

애스터리스크(\*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다.

다음 예제의 제너레이터 함수는 모두 유효하다.
하지만 일관성을 유지하기 위해 function 키워드 뒤에 붙이는 것을 권장한다.

```js
function* genFunc() { yield 1; }
function * genFunc() { yield 1; }
function *genFunc() { yield 1; }
function*genFunc() { yield 1; }
```

**제너레이터 함수는 화살표 함수로 정의할 수 없다.**

```js
const genArrowFunc = * () => {
	yield 1;
}; // SyntaxError
```

**new 연산자와 함께 생성자 함수로 호출할 수 없다.**

```js
function* genFunc() {
	yield 1;
}
new genFunc(); // TypeError
```

# 36.3 제너레이터 객체

**제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.**

제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.

제너레이터 객체는 next 메서드를 가지는 이터레이터이므로 Symbol.iterator 메서드를 호출해서 별도의 이터레이터를 생성할 필요가 없다.

```js
// 제너레이터 함수
function* genFunc() {
	yield 1;
	yield 2;
	yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```

제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 return, throw 메서드를 갖는다.
제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.

- next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```js
function* genFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		console.error(e);
	}
}
const generator = genFunc();

console.log(generator.next()); // {value:1, done: false}
console.log(generator.return('End!')); // {value: "End!", done: true}
```

- throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 donw 프로퍼티 값으로 갖는 리절트 객체를 반환한다.

```js
function* genFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		console.error(e);
	}
}
const generator = genFunc();

console.log(generator.next()); // {value:1, done: false}
console.log(generator.throw('Error!')); // {value: undefined, done: true}
```

# 46.4 제너레이터의 일시 중지와 재개

제어권을 함수 호출자에게 양도하여 필요한 시점에 함수 실행을 재개할 수 있다.

단, 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행한다.
**yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**

```js
function* genFunc() {
	yield 1;
	yield 2;
	yield 3;
}

const genrator = genFunc();
console.log(generator.next()); // {value:1, done: false}
console.log(generator.next()); // {value:2, done: false}
console.log(generator.next()); // {value:3, done: false}
console.log(generator.next()); // {value:undefined, done: true}
```

제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지(suspend)된다.
이때 함수의 제어권이 호출자로 양도(yield)된다.

**제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.**
yield 표현식을 할당받는 변수에 yield 표현식의 평가 결과가 할당되지 않는 것에 주의하자.

```js
function* genFunc() {
/*
	처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
	이때 yield 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
	x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
*/
	const x = yield 1;
/*
	두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는
	x 변수에 할당된다. 즉, const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료된다.
	두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
	이때 yield된 값 x+10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
*/
	const y = yield (x + 10);
/*
	세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는
    y 변수에 할당된다.
    즉, const y = yield (x + 10);는 세 번째 next 메서드를 호출했을 때 완료된다.
    세 번째 next 메서드를 호출하면 함수 끝까지 실행된다.
    이때 제너레이터 함수의 반환값 x + y 는 next 메서드가 반환한 이터레이터 리절트 객체의
    value 프로퍼티에 할당된다. 일반적으로 제너레이터의 반환값은 의미가 없다.
    따라서 제너레이터에는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야한다.
*/
	return x + y;
}
const generator = genFunc(0);
// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10);
console.log(res); // {value: 20, done: false}

res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

이처럼 next 메서드와 yield 표현식을 통해 함수 호출자와 함수 상태를 주고받을 수 있다.

함수 호출자는 next 메서드를 통해 yield 표현식까지 함수를 실행시켜 제너레이터 객체가 관리하는 상태(yield된 값)를 꺼내올 수 있고,
next 메서드에 인수를 전달해서 제너레이터 객체에 상태(yield 표현식을 할당받는 변수)를 밀어넣을 수 있다.

이처럼 제너레이터의 특성을 활용하면 비동기 처리를 동기처리처럼 구현할 수 있다.

# 46.5 제너레이터의 활용

## 46.5.1 이터러블의 구현

제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.
먼저 이터레이션 프로토콜을 준수하여 무한 피보나치 수열을 생성하는 함수를 살펴보자.

```js
const infinityeFibonacci = (function () {
	let [pre, cur] = [0, 1];
	return {
		[Symbol.iteraotr]() { return this; }
		next() {
			[pre, cur] = [cur, pre + cur];
			// 무한 이터러블이므로 done 프로퍼티를 생략한다.
			return { value: cur };
		}
	}
}());

// infiniteFibonacci는 무한 이터러블이다.
for(const num of infiniteFibonacci) {
	if(num > 10000) break;
	console.log(num); // 1 2 3 5 ... 2584
}
```

제너레이터를 사용하여 무한 피보나치 수열을 생성하는 함수를 구현해보자.

```js
const infiniteFibonacci = (function* () {
	let [pre, cur] = [0, 1]
	while(true) {
		[pre, cur] = [cur, pre + cur];
		yeild cur;
	}
}());

for(const num of infiniteFibonacci) {
	if(num > 10000) break;
	console.log(num); // 1 2 3 5 ... 2584
}
```

## 46.5.2 비동기 처리

제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처럼구현할 수 있다.

다시말해 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```js
// node-fetch : Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지
const fetch = require('node-fetch');

const async = generatorFunc => {
	const generator = generatorFunc(); // 2
	
	const onResloved = arg => {
		const result = generator.next(arg); // 5
		return result.done
			? result.value // 9
			: result.value.then(res => onResolved(res)); // 7
	};

	return onResolved; // 3
}

(async(function* fetchTodo() { // 1
	const url = 'https://~~'
	const response = yield fetch(url); // 6
	const todo = yield response.json(); // 8
	console.log(todo);
})()); // 4
```

위 예제는 다음과 같이 동작한다.

1. async 함수가 호출되면 전달받은 제너레이터 함수 fetchTodo를 호출하여 제너레이터 객체를 생성하고 onResolved 함수를 반환(3)한다. onResolved 함수는 상위 스코프의 generator 변수를 기억하는 클로저다. async 함수가 반환한 onResvoled 함수를 즉시 호출(4)하여 (2) 에서 생성한 제너레이터 객체의 next 메서드를 처음 호출(5)한다.
2. next 메서드가 처음 호출(5) 되면 제너레이터 함수 fetchTodo의 첫 번째 yield문(6)까지 실행된다. 이때 next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false 즉, 아직 제너레이터 함수가 끝까지 실행되지 않았따면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 첫 번째 yield된 fetch 함수가 반환한 프로미스가 resovle한 Response 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출(7)한다.
3. onResolved 함수에 인수로 전달된 Response 객체를 next 메서드에 인수로 전달하면서 next 메서드를 두 번째로 호출(5)한다. 이때 next 메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo 의 response 변수(6)에 할당되고 제너레이터 함수 fetchTodo의 두 번째 yield문(8)까지 실행된다.
4. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false, 즉 아직 제너레이터 함수 fetchTodo가 끝까지 실행되지 않았따면 이터레이터 리절트 객체의 value 프로퍼티 값, 즉 두번째 yield된 response.json 메서드가 반환한 프로미스가 resolve한 todo 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출(7)한다.
5. onREsolved 함수에 인수로 전달된 todo 객체를 next 메서드에 인수로 전달하면서 next 메서드를 세 번째로 호출(5)한다. 이때 next 메서드에 인수로 전달한 todo 객체는 제너레이터 함수 fetchTodo의 todo 변수(8)에 할당되고 제너레이터 함수 fetchTodo가 끝까지 실행된다.
6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true, 즉 제너레이터 함수 fetchTodo가 끝까지 실행되었다면 이터레이터 리절트 객체의 value프로퍼티 값, 즉 제너레이터 함수 fetchTodo의 반환값인 undefined를 그대로 반환(9)하고 처리를 종료한다.

async 함수는 이해를 돕기 위해 간략화한 예제이므로 완벽하지 않다.
async / await 를 사용하면 위와 같은 제너레이터 실행기를 사용할 일이 없지만 혹시 제너레이터 실행기가 필요하다면 직접 구현하는 것보다 co 라이브러리를 사용하기 바란다.

```js
const fetch = require('node-fetch');

const co = require('co');

co(function* fetchTodo() {
	const url = 'https://~'
	const response = yield fetch(url);
	const todo = yield response.json();
	console.log(todo);
});
```

# 46.6 async/await

제너레이터를 사용해서 비동기 처리를 동기처럼 동작하도록 구현했지만 코드가 무척이나 장황해지고 가독성도 나빠졌다. ES8(ECMAScript 2017)에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입되었다.

async/await는 프로미스를 기반으로 동작한다.

## 46.6.1 async 함수

await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
async 함수는 언제나 프로미스를 반환한다.
async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환하는 resolve하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) {return n;}
foo(1).then(v => console.log(v));

// async 함수 표현식
const bar = async function(n) {return n;}
bar(2).then(v => console.log(v));

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v));

// async 메서드
const obj = {
	async foo(n) {return n;}
}
obj.foo(4).then(v => console.log(v));

// async 클래스 메서드
class MyClass {
	async bar(n) {return n;}
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v));
```

클래스의 constructor 메서드는 async 메서드가 될 수 없다.
클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환해야 한다.

## 46.6.2 await 키워드

await 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 떄까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다. await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개한다.

```js
async function foo() {
	const a = await new Promise(res => setTimeout(() => res(1), 3000))
	const b = await new Promise(res => setTimeout(() => res(2), 2000))
	const c = await new Promise(res => setTimeout(() => res(3), 1000))
	console.log([a,b,c]); // [1,2,3]
}
foo(); // 약 6s 소요된다.
```

모든 프로미스에 await 키워드를 사용하는 것은 주의해야 한다.
3개의 비동기 처리는 서로 연관이 없이 개별적으로 수행되는 비동기 처리이므로 순차적으로 처리할 필요가 없다.
따라서 foo는 다음과 구현하는 것이 좋다.

```js
async function foo() {
	const res = await Promise.all([
		new Promise(res => setTimeout(() => res(1), 3000)),
		new Promise(res => setTimeout(() => res(2), 2000)),
		new Promise(res => setTimeout(() => res(3), 1000))
	])
	console.log([a,b,c]); //[1,2,3]
}
foo(); // 약 3s 소요된다.
```

## 46.6.3 에러 처리

async/await 에서 에러처리는 try...catch 문을 사용할 수 있다.
콜백 함수를 인수로 전달받는 비동기와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.

```js
const foo = async () => {
	try {
		const wrongUrl = 'https:://~~'
		const response = await fetch(wrongUrl);
		const data = await response.json();
		console.log(data);
	}
	catch(e) {
		console.error(e);
	}
}

foo();
```

**async 함수 내에서 catch문을 사용해서 에러처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.**

```js
const foo = async () => {
	const wrongUrl = 'https:://~~'
	const response = await fetch(wrongUrl);
	const data = await response.json();
	return data;
}

foo()
	.then(console.log)
	.catch(console.error) // TypeError: Failed to fetch
```

# 알고 가기
---
- [ ] 제너레이터에 대해 설명해주세요.
