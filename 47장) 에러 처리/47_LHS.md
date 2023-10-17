# 47.1 에러 처리의 필요성

try...catch 문을 해 발생한 에러에 적적하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.

```js
try {
	foo()
} catch(error) {
	console.error('[에러발생]', error)
}
// 프로그램이 강제 종료되지 않는다.
```

직접적으로 에러를 발생하지 않는 예외(exception)적인 상황이 발생할 수도 있다.
예외적인 상황에 적절하게 대응하지 않으면 에러로 이어질 가능성이 크다.

# 47.2 try... catch... finally 문

에러 처리를 구현하는 방법은 크게 두 가지가 있다.
1. 예외적인 상황이 발생하면 반환하는 값(null 또는 -1)을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리는 방법
2. 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법

try...catch...finally 문은 두 번째 방법이다.
이 방법을 에러 처리(error handling)이라 한다.

```js
try {
	// 실행할 코드(에러가 발생할 가능성이 있는 코드)
}
catch (err) {
	// try 코드 블록에서 에러가 발생하면 이 코드의 블록의 코드가 실행된다.
	// err 에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
}
finally {
	// 에러 발생과 상관없이 반드시 한 번 실행된다.
}
```

# 47.3 Error 객체

Error 생성자 함수는 에러 객체를 생성한다.
Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다.

```js
const error = new Error('invalid')
```

Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.
message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이고,
stack 프로퍼티의 값은 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용한다.

자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공한다.
SyntaxError, ReferenceError, TypeError, RagneError, URIError, EvalError 생성자 함수가 생성한 에러 객체의 프로토타입은 모두 Error.prototype을 상속받는다.

| 생성자 함수    | 인스턴스                                                                       |
| -------------- | ------------------------------------------------------------------------------ |
| Error          | 일반적 에러 객체                                                               |
| SyntaxError    | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체                |
| ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                         |
| TypeError      | 피연산산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체       |
| RangeError     | 숫자값 허용 범위를 벗어났을 때 발생하는 에러 객체                              |
| URIError       | encodeURI 또는 decodeuRI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
| EvalError      | eval 함수에서 발생하는 에러 객체                                               |

# 47.4 throw 문

Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
즉 에러 객체 생성과 에러 발생은 의미가 다르다.

```js
try {
	new Error('something wrong');
} catch (error) {
	console.log(error);
}
```

에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다.

```text
throw 표현식;
```

thorw 문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 지정한다.
에러를 던지면 catch문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다.

그리고 catch 코드 블록이 실행되기 시작한다.

```js
try {
	throw new Error('something wrong');
} catch (error) {
	console.log(error);
}
```

```js
const repeat = (n, f) => {
	if(typeof f !== 'function') throw new TypeError('f must be a function');
	// ...
}

try {
	// repeat 함수는 에러를 발생시킬 가능성이 있으므로 try 코드 블록 내부에서 호출해야한다.
	repeat(2, 1);
} catch (err) {
	console.error(err); // TypeError: f must be a function
}
```

# 47.5 에러의 전파

에러는 호출자 방향으로 전파된다.
콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

```js
const foo = () => {
	throw Error('foo error'); // 4
}
const bar = () => {
	foo(); // 3
}
const baz = () => {
	bar(); // 2
}
try {
	baz(); // 1
} catch(e) {
	console.error(e)
}
```

1 에서 baz 함수를 호출하면
2 에서 bar 함수가 호출되고
3 에서 foo 함수가 호출되고
foo 함수는 4 에서 에러를 throw 한다.

이때 foo 함수가 throw한 에러는 다음과 같이 호출자에게 전파되어 전역에서 캐치된다.

![[Pasted image 20231015203110.png]]

이처럼 throw 된 에러를 캐치하지 않으면 호출자 방향으로 전파된다.

주의할 것은 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것이다.
setTimeout 이나 프로미스 후속 처리 메서드의 콜백 함수는 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의해 콜스택으로 푸시되어 실행된다.
이때 콜스택에 푸시된 콜백함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재하게 된다.
따라서 에러를 전파할 호출자가 존재하지 않는다.

# 알고 가기
---
- 생략
