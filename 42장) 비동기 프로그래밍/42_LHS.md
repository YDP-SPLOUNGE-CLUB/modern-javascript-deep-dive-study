# 42.1 동기 처리와 비동기 처리

실행 컨텍스트에서 살펴본 바와 같이
함수를 호출하면 함수 코드가 평가되어 함수 실행 컨텍스트가 생성된다.

이때 생성된 함수 실행 컨텍스트는 실행 컨텍스트 스택(콜스택)에 푸시되고 함수 코드가 실행된다.

함수 코드의 실행이 종료하면 함수 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.

```js
const foo = () => {};
const bar = () => {};

foo();
bar();
```

![[Pasted image 20231002161255.png]]

이처럼 함수의 실행 순서는 실행 컨텍스트 스택으로 관리한다.

**자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다.**
- 함수를 실행할 수 있는 창구가 단하나임을 의미
- 동시에 2개 이상의 함수를 동시에 실행할 수 없다는 것을 의미

실행 컨텍스트 스택의 최상위 요소인 "실행 중인 실행 컨텍스트"를 제외한 모든 실행 컨텍스트는 모두 실행 대기중인 태스크들이다.
대기중인 태스크들은 현재 실행 중인 실행 컨택스트가 팝되어 실행 컨텍스트 스택에서 제거되면, 다시 말해 현재 실행 중인 함수가 종료하면 비로소 실행되기 시작한다.

이처럼 자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 **싱글스레드** 방식으로 동작한다.
싱글 스레드 방식

한 번에 하나의 태스크만 실행할 수 있기 때문에 처리에 시간이 걸리는 태스크를 실행하는 경우 **블로킹(작업중단/blocking)이 발생**한다.

현재 실행중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을 **동기(synchronous)처리**라고 한다.

```js
function sleep(func, delay) {
	const delayUntil = Date.now() + delay;
	while (Date.now() < delayUntil) ;
	func();
}
function foo() {}
function bar() {}
sleep(foo, 3 * 1000) // 3초 경과 후 foo 호출 -> bar 호출
```

동기 처리 방식은 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장된다는 장점이 있지만 앞선 태스크가 종료할 떄까지 이후 태스크들이 블로킹되는 단점이 있다.

```js
function foo() {}
function bar() {}

setTimout(foo, 3 * 1000);
bar(); // bar 호출 -> 3초 경과후 foo 호출
```

setTimout 함수 이후의 태스크를 블로킹하지 않고 곧바로 실행한다.

이처럼 현재 실행 중인 태스크가 종료되지 않은 상태라 해도
다음 태스크를 곧바로 실행하는 방식을 **비동기(asynchronous) 처리**라고 한다.

비동기 처리 방식은 현재 실행 중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하므로 블로킹이 발생하지 않는다는 장점이 있찌만, 태스크의 실행 순서가 보장되지 않는 단점이 있다.

비동기 처리를 수행하는 비동기함수는 전통적으로 콜백 패턴을 사용한다.
콜백 패턴은 콜백 헬을 발생시켜 가독성을 나쁘게 하고,
비동기 처리 중 발생한 에러의 예외 처리가 곤란하며,
여러 개의 비동기 처리를 한 번에 처리하는 데도 한계가 있다.

**타이머 함수인 setTimout 과 setInterval HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.**
비동기 처리는 이벤트 루프와 테스크 큐와 깊은 관계가 있다.

# 42.2 이벤트 루프와 태스크 큐

자바스크립트의 특징 중 하나는 싱글 스레드로 동작한다는 것이다.
싱글 스레드는 하나의 태스크만 처리할 수 있음을 의미한다.
브라우저가 동작하는 것을 살펴보면 태스크가 동시에 처리되는 것처럼 느껴진다.

예를들어 HTML 요소가 애니메이션 효과를 통해 움직이며 이벤트를 처리하고, HTTP 요청을 통해 서버로부터 데이터를 가지고 오면서 렌더링하기도 한다.
이처럼 자바스크립트의 **동시성(concurrency)** 을 지원하는 것이 바로 **이벤트 루프(event loop)** 다.

이벤트 루프는 브라우저에 내장되어 있는 기능 중 하나다.

![[Pasted image 20231002163453.png]]

구글의 V8 자바스크립트 엔진을 비롯한 대부분의 자바스크립트 엔진은 크게 2개의 영역으로 구분할 수 있다.

### 콜 스택(call stack)
-  소스코드(전역 코드나 함수코드 등) 평가 과정에서 생성된 실행 컨텍스트가 추가되고 제거되는 스택 자료구조인 실행 컨텍스트 스택이 바로 콜스택이다.
- 함수를 호출하면 함수 실행 컨텍스트가 순차적인 콜 스택에 푸시되어 순차적으로 실행된다. 자바스크립트 엔진은 단 하나의 콜스택을 사용하기 때문에 최상위 실행 컨텍스트(실행중인 실행 컨텍스트)가 종료되어 콜스택에서 제거되기 전까지는 다른 어떤 태스크도 실행되지 않는다.
### 힙 (heap)
-  힙은 객체가 저장되는 메모리 공간이다. 콜스택의 요소인 실행 컨텍스트는 힙에 저장된 객체를 참조한다.
- 메모리에 값을 저장하려면 먼저 값을 저장할 메모리 공간의 크기를 결정해야 한다. 객체는 원시 값과는 달리 크기가 정해져 있지 않으므로 할당해야 할 메모리 공간의 크기를 런타임에 결정(동적 할당)해야 한다. 따라서 객체가 저장되는 메모리 공간인 힙은 구조화 되어 있지 않다는 특징이 있다.

이처럼 콜 스택과 힙으로 구성되어 있는 자바스크립트 엔진은 단순히 태스크가 요청되면 콜 스택을 통해 요청된 작업을 순차적으로 실행할 뿐이다.

**비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리는 자바스크립트 엔진을 구동하는 환경인 브라우저 또는 Node.js가 담당한다.**

예를들어 setTimout 의 콜백 함수의 평가와 실행은 자바스크립트 엔진이 담당하지만
호출 스케줄링을 위한 타이머 설정과 콜백 함수의 등록은 브라우저 또는 Node.js가 담당한다.

이를 위해 브라우저 환경은 태스크 큐와 이벤트 루프를 제공한다.

### 테스크 큐 (task queue / event queue / callback queue)
- setTimout 이나 setInterval 과 같은 **비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역**이다. 태스크 큐와는 별도로 프로미스 후속 처리 메서드의 콜백 함수가 일시적으로 보관되는 마이크로태스크 큐도 존재한다.
### 이벤트 루프 (event loop)
- 이벤트 루프는 콜 스택에 현재 실행 중인 실행 컨텍스트가 있는지, 그리고 태스크 큐에 대기중인 함수(콜백함수, 이벤트 핸들러 등)가 있는지 반복해서 확인한다.
- 만약 **콜 스택이 비어있고 태스크 큐에 대기 중인 함수가 있다면 이벤트 루프는 순차적(FIFO)으로 태스크 큐에 대기중인 함수를 콜 스택으로 이동시킨다.** 이때 콜 스택으로 이동한 함수는 실행된다. 즉 태스크 큐에 일시 보관된 함수들을 비동기 처리 방식으로 동작한다.

## 브라우저 환경에서 어떻게 동작할까?

```js
function foo() {
	console.log('foo');
}
function bar() {
	console.log('bar');
}

setTimeout(foo, 0); // 0초 실제는 4ms 후에 foo 함수가 호출된다.
bar();
```

1. 전역 코드가 평가되어 **전역 실행 컨텍스트가 생성되고 콜 스택에 푸시**된다.
2. 전역 코드가 실행되기 시작하여 **setTimeout 함수가 호출**된다.
    - 이때 **setTimeout 함수의 함수 실행 컨텍스트가 생성**되고 **콜 스택에 푸시**되어 **현재 실행 중인 실행 컨텍스트가 된다**. 브라우저의 **Web API(호스트 객체)인 타이머 함수**도 함수이므로 **함수 실행 컨텍스트를 생성**한다.
3. setTimeout 함수가 실행되면 **콜백 함수를 호출 스케줄링하고 종료**되어 **콜 스택에서 팝**된다.
    - 이때 **호출 스케줄링**, 즉 타이머 설정과 타이머가 만료되면 **콜백 함수를 태스크 큐에 푸시하는 것은 브라우저의 역할**이다.
4. **브라우저가 수행**하는 4-1과 **자바스크립트 엔진이 수행**하는 4-2는 **병렬 처리**된다.
    1. 브라우저는 타이머를 설정하고 **타이머의 만료를 기다린다**. 이후 타이머가 만료되면 **콜백 함수 foo가 태스크 큐에 푸시**된다. 위 예제의 경우 지연 시간(delay) 4ms 후에 콜백 함수 foo 가 태스크 큐에 푸시되어 대기하게 된다. 이 처리 또한 자바스크립트 엔진이 아니라 브라우저가 수행한다. 이처럼 setTimeout 함수로 호출 스케줄링한 콜백 함수는 정확히 지연시간 후에 호출된다는 보장은 없다. 지연 시간 이후에 콜백 함수가 태스크 큐에 푸시되어 대기하게 되지만 콜 스택이 비어야 호출되므로 약간의 시간차가 발생할 수 있기 때문이다.
    2. **bar 함수가 호출되어 bar 함수의 함수 실행 컨텍스트가 실행**되고 **콜 스택에 푸시**되어 **현재 실행중인 실행 컨텍스트가 된다.** 이후 **bar 함수가 종료되어 콜 스택에서 팝**된다. 이때 브라우저가 타이머를 설정한 후 4ms가 경과했다면 foo 함수는 아직 태스크 큐에서 대기중이다.
5. **전역 코드 실행이 종료**되고 **전역 실행 컨텍스트가 콜 스택에서 팝**된다. 이로서 콜 스택에는 아무런 실행 컨텍스트도 존재하지 않게 된다.
6. **이벤트 루프에 의해 콜스택이 비어 있음이 감지**되고 태스크 큐에서 대기중인 콜백 함수 foo가 이벤트 루프에 의해 **콜스택에 푸시**된다. 다시말해 콜백함수 foo의 함수 실행 컨텍스트가 생성되고 콜 스택에 푸시되어 현재 실행 중인 실행 컨텍스트가된다.  **이후 foo 함수가 종료되어 콜스택에서 팝**된다.

이처럼 비동기 함수인 setTimeout 의 콜백 함수는 태스크 큐에 푸시되어 대기하다가 콜 스택이 비게되면, 다시 말해 전역 코드 및 명시적으로 호출된 함수가 모두 종료하면 비로소 콜 스택에 푸시되어 실행된다.

자바스크립트는 싱글 스레드 방식으로 동작한다.
이때 싱글 스레드 방식으로 동작하는 것은 브라우저가 아니라 브라우저에 내장된 자바스크립트 엔진이라는 것에 주의하기 바란다.
만약 모든 자바스크립트 코드가 자바스크립트 엔진에서 싱글 스레드 방식으로 동작한다면 자바스크립트는 비동기로 동작할 수 없다.
**즉 자바스크립트 엔진은 싱글 스레드로 동작하지만 브라우저는 멀티 스레드로 동작한다.**

# 알고 가기
---
- [ ] 다음 코드를 자바스크립트와 브라우저의 관점에서 설명해주세요.
```js
function foo() {
	console.log('foo');
}
function bar() {
	console.log('bar');
}

setTimeout(foo, 0);
bar();
```
