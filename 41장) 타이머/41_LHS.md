# 41.1 호출 스케줄링

자바스크립트는 setTimeout, setInterval, clearTimeout, clearInterval 을 제공한다.
타이머 함수는 ECMAScript 사양에 정의된 빌트인 함수가 아니다. 하지만 모두 전역 객체의 메서드로서 타이머함수를 제공한다.
즉 타이머 함수는 호스트 객체다.

자바스크립트 엔진은 단하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없다.

즉 자바스크립트 엔진은 **싱글 스레드**로 동작한다.
이런 이슈로 타이머 함수는 **비동기 처리 방식**으로 동작한다.

# 41.2 타이머 함수
## 41.2.1 setTimeout / clearTimeout

단 한번 동작하는 타이머를 생성한다., 이후 타이머가 만료되면 첫 번째 인수로 전달한 콜백 함수가 호출된다.

```text
const timeoutId = setTimeout(func|code[, delay, param1, param2, ...]);
```

| 매개 변수           | 설명                                                                                                                                                                                                                                                                              |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| func                | 타이머가 먄료된 뒤 호출될 콜백 함수<br>- 콜백함수 대신 코드를 문자열로 전달할 수 있다. 이때 코드 문자열은 타이머가 만료된 뒤 해석되고 실행된다. 이는 흡사 eval 함수와 유사하며 권장되지 않는다.                                                                                   |
| delay               | 타이머가 만료 시간(ms)  setTimeout 함수는 delay 시간으로 단 한번 동작하는 타이머를 생성한다. 인수 전달을 생략한 경우 기본값으로 0이 지정된다.<br>- delay 시간은 태스크 큐에 콜백 함수를 등록하는 시간을 지연할 뿐이다.<br>- delay가 4ms 이하인 경우 최소 지연시간 4ms가 지정된다. |
| param1, param2, ... | 호출 스케줄링된 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후로 인수로 전달할 수 있다. (IE9 이하에서는 콜백 함수에 인수를 전달할 수 없다.)                                                                                                                            |


```js
setTimeout(() => console.log('Hi!'), 1000);
setTimeout((name) => console.log('Hi!' + name), 1000, 'Lee');
```

setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
setTImeout 함수가 반환한 타이머 id는 브라우저 환경인 경우 숫자이며 Node.js 환경인 경우 객체이다.

id를 clearTimeout 함수의 인수로 전달하며 타이머를 취소할 수 있다.
즉 clearTimeout 함수는 호출 스케줄링을 취소한다.

```js
const timerId = setTimeout(() => console.log('hi'), 1000);
clearTimeout(timerId);
```

## 41.2.2 setInterval / clearInterval

반복 동작하는 타이머를 생성한다, 이후 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다. 이는 타이머가 취소될 때까지 계속된다.

```text
const timerId = setInterval(func|code,[, delay, param1, param2, ...]);
```

타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
브라우저 환경에서는 숫자이며 Node.js 환경에서는 객체이다.

setInterval 함수가 반환한 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다.

```js
const count = 1;
const timeoutId = setInterval(() => {
	console.log(count); // 1 2 3 4 5
	if(count ++ === 5) clearInterval(timeoutId);
}, 1000)
```

# 41.3 디바운스와 스로틀

scroll, resize, input, mousemove 같은 이벤트는 짧은 시간 간격으로 연속해서 발생한다.
디브운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

```js
const debounce = (cb, delay) => {
	let timerId;
	return event => {
		if(timerId) clearTimeout(timerId);
		timerId = setTimeout(cb, delay, event);
	};
}
```

```js
const throttle = (cb, delay) => {
	let timerId;
	return event => {
		if(timerId) return;
		timerId = setTimeout(() => {
			cn(event);
			timerId = null;
		}, delay, event)
	}
}
```

## 41.3.1 디바운스

디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다.

즉 디바운스는 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한 번만 이벤트 핸들러가 호출되도록 한다.

resize 이벤트 처리나 input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리등에 유용하게 사용된다.

위 예제는 debounce 함수는 이해를 위해 간략하게 구현하여 완전하지 않다.
실무에서는 Underscore의 debounce나 Lodash의 debounce 함수를 사용하는 것을 권장한다.

## 41.3.2 스로틀

스로틀은 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다.
즉 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

scroll 이벤트는 사용자가 스크롤 할 때 잛은 시간 간격으로 연속해서 발생한다.
이처럼 짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 스로틀 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

스로틀 함수가 반환한 함수는 스로틀 함수에 두 번째 인수로 전달한 시간(delay)이 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가 delay 시간이 경과했을때 이벤트가 발생하면 콜백함수를 호출하고 새로운 타이머를 재설정한다.
따라서 delay 시간 간격으로 콜백 함수가 호출된다.

이처럼 스로틀은 scroll 이벤트 처리나 무한스크롤 UI 구현에 유용하게 사용된다.
실무에서는 Underscore의 throttle나 Lodash의 throttle 함수를 사용하는 것을 권장한다.

# 알고 가기
---
- 생략
