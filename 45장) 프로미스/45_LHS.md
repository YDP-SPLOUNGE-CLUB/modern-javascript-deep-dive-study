ES6에서 비동기 처리를 위한 또 다른 패턴으로 프로미스를 도입했다.

프로미스는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.

# 45.1 비동기  처리를 위한 콜백 패턴의 단점

## 45.1.1 콜백 헬

```js
const get = url => {
	const xhr = new XMLHttpRequest();
	xhr.open('GET', url);
	xhr.send();
	xhr.onload = () => {
		if(xhr.status === 200) {
			console.log(JSON.parse(xhr.response));
		} else {
			console.log('Error', xhr.status, xhr.statusText);
		}
	}
}
```

get 함수는 비동기 함수다.
비동기 함수란  내부에 비동기로 동작하는 코드를 포함한 함수를 말한다.

**비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다. 즉 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.
따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.**

예를들어 비동기함수인 setTimeout 함수의 콜백 함수는 setTimeout 함수가 종료된 이후에 호출된다.
따라서 setTimeout 함수 내부의 콜백 함수에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

setTimeout 함수의 콜백 함수에서 상위 스코프의 변수에 값을 할당해보자.
setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환하므로 콜백 함수에서 값을 반환하는 것은 무의미하다.

```js
let g = 0;
// setTimeout 함수는 콜백 함수의 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하지 못한다.
setTimout(() => { g = 100; }, 0);
console.log(g); // 0
```

get 함수가 비동기 함수인 이유는 **get 함수 내부의 onload 이벤트 핸들러가 비동기로 동작**하기 때문이다.

get 함수를 호출하면 GET 요청을 전송하고 onload 이벤트 핸들러를 등록한 다음 undefined를 반환하고 즉시 종료된다.

```js
const get = url => {
	const xhr = new XMLHttpRequest();
	xhr.open('GET', url);
	xhr.send();
	xhr.onload = () => {
		if(xhr.status === 200) {
			return JSON.parse(xhr.response);
		} else {
			console.log('Error', xhr.status, xhr.statusText);
		}
	}
}
const response = get('http://~');
console.log(response); // undefined
```

get 함수가 호출되면 XMLHttpRequest 객체를 생성하고, HTTP 요청을 초기화한 후, HTTP 요청을 전송한다.
xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하고 종료한다.
이때 get 함수에 명시적인 반환문이 없으므로 get 함수는 undefined를 반환한다.

이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러의 반환문은 get 함수의 반환문이 아니다.
get 함수는 반환문이 생략되었으므로 암묵적으로 undefined를 반환한다.

```js
document.querySelector('input').oninput = function () {
	console.log(this.value);
	// 이벤트 핸들러의 반환은 의미가 없다.
	return this.value;
}
```

상위 스코프의 변수에 할당하여도 결과는 같다.

```js
const todos;
const get = url => {
	const xhr = new XMLHttpRequest();
	xhr.open('GET', url);
	xhr.send();
	xhr.onload = () => {
		if(xhr.status === 200) {
			todos = JSON.parse(xhr.response);
		} else {
			console.log('Error', xhr.status, xhr.statusText);
		}
	}
}
const response = get('http://~');
console.log(response); // undefined
```

onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 언제나 console.log가 종료한 이후 호출된다.

이유는 다음과 같다.
1. 비동기 함수 get이 호출되면 함수 코드를 평가하는 과정에서 get 함수의 실행 컨텍스트가 생성되며 컨텍스트 스택(콜 스택)에 푸시된다.
2. 함수 코드 실행과정에서 xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러가 바인딩된다.
3. get 함수가 종료하면 get 함수의 실행 컨텍스트가 콜 스택에서 팝된다.
4. conosle.log가 호출되며, console.log의 실행 컨텍스트가 생성되어 실행 컨텍스트 스택에 푸시된다.
   만약 console.log 가 호출 전 load 이벤트가 발생했더라도 결코 console.log 보다 먼저 실행되지 않는다.
5. 서버로부터 응답이 도착하면 xhr 객체에서 load 이벤트가 발생한다.
6. xhr.onload 핸들러 프로퍼티에 바인딩한 이벤트가 즉시 실행되는 것이 아니라 load 이벤트가 발생하면 일단 테스크 큐에 저장되어 대기한다.
7. 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.

이벤트 핸들러도 함수이므로
`이벤트 핸들러의 평가` -> `이벤트 핸들러의 실행 컨텍스트 생성` -> `콜 스택에 푸시` -> `이벤트 핸들러 실행 과정`
을 거친다.

때문에 비동기함수는 다음과 같다.
- **비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없다.**
- **상위 스코프의 변수에 할당할 수 없다.**

따라서 **비동기 함수의 처리 결과에 대한 후속처리는 비동기 함수 내부에서 수행**해야 한다.

이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한
**후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적**이다.

필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와
비동기 처리가 실패하면 호출될 콜백함수를 전달할 수 있다.

```js
const get = (url, successCb, failureCb) => {
	xhr.open('GET', url);
	xhr.send();
	xhr.onload = () => {
		if(xhr.status === 200) {
			successCb(JSON.parse(xhr.response));
		} else {
			failureCb(xhr.status)
		}
	}
}

get('https://~', console.log, console.error);
/*
{
	"user": 1,
	...
}
*/
```

이처럼 비동기 함수가 비동기 처리 결과를 가지고 또 다시 비동기 함수를 호출해야 한다면
콜백함수의 호출이 중첩되어 복잡도는 높아진다.
이를 콜백헬 이라 한다.

```js
get('/step1', a => {
	get(`/step2${a}`, b => {
		get(`/step3${b}`, c => {
			console.log(d)
		})
	})
})
```

## 45.1.2 에러 처리의 한계

비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러처리가 곤란하다는 것이다.

```js
try {
	setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
	// 에러를 캐치하지 못한다.
	console.error('error', e)
}
```

**이유는 다음과 같다.**

1. 비동기 함수인 setTimeout 호출된다.
2. setTImeout 함수의 실행 컨텍스트가 생성된다.
3. 콜 스택에 푸시되어 실행에서 제거된다.
4. 타이머가 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시된다.
5. 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.
   
   setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태다.
   이것은 setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니라는 것을 의미한다.

setTimeout 함수의 콜백 함수의 호출자가 setTimeout 함수라면 콜 스택의 현재 실행 중인 실행 컨텍스트가 콜백 함수의 실행 컨텍스트일 때 현재 실행 중인 실행 컨텍스트의 하위 실행 컨텍스트가 setTimeout 함수여야 한다.

**에러는 호출자 방향으로 전파된다.**
즉 콜스택의 아래 방향(실행중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

앞에서 살펴본바와 같이 setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다.
따라서 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치되지 않는다.

이를 극복하기 위해 ES6에서 프로미스가 도입되었다.

# 45.2 프로미스의 생성

Promise는 호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체다.

```js
const promise = new Promise((resolve, reject) => {
	if(처리성공) {
		resolve('result');
	} else {
		reject('failure reason');
	}
})
```

Promise 로 다시 비동기함수 get을 구현하면 다음과 같다.

```js
const promiseGet = url => {
	return new Promise((resolve, reject) => {
		xhr.open('GET', url);
		xhr.send();
		xhr.onload = () => {
			if(xhr.status === 200) {
				resolve(JSON.parse(xhr.response));
			} else {
				reject(xhr.status)
			}
		}
	})
}
// 프로미스를 반환한다.
promiseGet('https://~')
```

프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.

| 프로미스의 상태 정보 | 의미                                  | 상태 변경 조건                   |
| -------------------- | ------------------------------------- | -------------------------------- |
| pending              | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
| fulfilled            | 비동기 처리가 수행된 상태(성공)       | resolve 함수 호출                |
| rejected             | 비동기 처리가 수행된 상태(실패)       | reject 함수 호출                 |

**이처럼 promise는 resolve 또는 reject 함수를 호출하는 것으로 결정된다.**

fulfilled 또는 rejected 상태를 settled 상태라고 한다.
settld 상태는 fullfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다.
일단 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.

즉 **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.**

# 45.3 프로미스의 후속 처리 메서드

프로미스는 후속 메서드 then, catch, finally 를 제공한다.

**프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.**
후속 처리 메서드는 프로미스를 반환하며 비동기로 동작한다.

## 45.3.1 Promise.prototype.then

then 메서드는 두 개의 콜백 함수를 인수로 전달받는다.
- 첫 번째 콜백 함수
    - fullfilled 상태(resolve 함수 호출된 상태)가 되면 호출된다.
- 두 번째 콜백 함수
    - rejected 상태(reject 함수 호출된 상태)가 되면 호출된다.

```js
new Promise(res => res('fullfilled'))
	.then(v => console.log(v), e => console.log(e)); // fullfilled
new Promise((_, rej) => rej(new Error('rejected')))
	.then(v => console.log(v), e => console.log(e)); // Error rejected
```

then 메서드는 언제나 프로미스를 반환한다.

## 45.3.2 Promise.prototype.catch

catch 메서드는 한 개의 콜백 함수를 인수로 전달받는다.
프로미스가 rejected 상태인 경우만 호출된다.

```js
new Promise((_, rej) => rej(new Error('rejected')))
	.catch(e => console.log(e)); // Error rejected

// 동일하게 동작한다.
new Promise((_, rej) => rej(new Error('rejected')))
	.then(v => console.log(v), e => console.log(e)); // Error rejected
```

## 45.3.3 Promise.prototype.finally

finally 메서드는 한 개의 콜백 함수를 인수로 전달받는다.
성공, 실패와 상관없이 무조건 한 번만 호출된다.
또한 언제나 프로미스를 반환한다.

```js
new Promise(() => {})
	.finally(() => console.log('finally')); // finally
```

# 45.4 프로미스의 에러 처리

에러처리는 then, catch 메서드를 통해 처리할 수 있다.

```js
PromiseGet(url)
	.then(res => console.log(res))
	.then((undefined, err => console.error(err))); // Error

/*
	then 메서드의 두 번째 콜백 함수는
	첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해진다.
*/
PromiseGet(url).then(
	res => console.xxx(res),
	err => console.error(err)
)

/*
	catch 메서드를 모든 then 메서드 호출한 이후 호출하면
	비동기 처리에서 발생한 에러뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.
*/
PromiseGet(url)
	.then(res => console.xxx(res))
	.catch(err => console.error(err)); // TypeError
```

catch 메서드를 사용하는 것을 권장한다.

# 45.5 프로미스 체이닝

then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다.
이를 프로미스 체이닝이라고 한다.

```js
promiseGet(url)
	.then(({userId}) => promiseGet(`${url}/${userId}`))
	.then(userInfo => console.log(userInfo))
	.catch(err => console.error(err))
```

문제는 프로미스도 콜백 패턴을 사용하므로 가독성이 좋지 않다.

이 문제는 ES8의 async/await 를 통해 해결할 수 있다.

```js
(async () => {
	const {userId} = await promiseGet(url);
	const userInfo = await promiseGet(`${url}/${userId}`))
})
```

# 45.6 프로미스의 정적 메서드

Promise 는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있다.
**Promise는 5가지 정적 메서드를 제공한다.**

### 45.6.1 Promise.resolve / Promise.reject

이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.

Promise.resolve 메서드는 인수로 전달받은 값을 resolve 하는 프로미스를 생성한다.

```js
const resolvedPromise = Promise.resolve([1, 2, 3]);
resolvedPromise.then(console.log); // [1, 2, 3]

// 동일하게 동작한다.
const resolvedPromise = new Promise(resolve => resolve([1, 2, 3]));
resolvedPromise.then(console.log); // [1, 2, 3]

const rejectedPromise = Promise.reject([1, 2, 3]);
resolvedPromise.catch(console.log); // Error

// 동일하게 동작한다.
const rejectedPromise = new Promise((_,reject) => reject([1, 2, 3]));
resolvedPromise.reject(console.log); // Error
```

### 45.6.2 Promise.all

여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

```js
const req1 = () => new Promise(res => setTimeout(() => res(1), 3000));
const req2 = () => new Promise(res => setTimeout(() => res(2), 2000));
const req3 = () => new Promise(res => setTimeout(() => res(3), 1000));

const res = [];
// 세 개의 비동기 처리를 순차적으로 처리
req1()
	.then(data => {
		res.push(data);
		return req2();
	})
	.then(data => {
		res.push(data);
		return req3();
	})
	.then(data => {
		res.push(data)
		console.log(res); // [1, 2, 3] 약 6초 소요
	})
	.catch(console.error)
```

위 예제는 세 개의 비동기 처리를 순차적으로 처리한다. 3s -> 2s -> 1s
서로 의존하지 않고 개별적으로 수행된다.

병렬로 처리하려면 Promise.all 메서드를 사용한다.

```js
const req1 = () => new Promise(res => setTimeout(() => res(1), 3000));
const req2 = () => new Promise(res => setTimeout(() => res(2), 2000));
const req3 = () => new Promise(res => setTimeout(() => res(3), 1000));

Promise.all([req1(), req2(), req3()])
	.then(console.log); // [1, 2, 3] 약 3초 소요
```

Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블 인수로 전달받는다.

위 예제의 경우 다음과 같이 동작한다.
- 첫 번째 프로미스 : 3s 후에 1을 resolve
- 두 번째 프로미스 : 2s 후에 2을 resolve
- 세 번째 프로미스 : 1s 후에 3을 resolve

**Promise.all 메서드는 인수로 전달받은 배열의 모든 프로미스가 fulfilled 상태가 되면 종료한다.**
Promise.all 메서드가 종료하는데 걸리는 시간은 가장 늦게 fullfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 길다.

모든 프로미스가 fullfilled 상태가 되면 resolve 된 처리 결과를 모두 배열에 저장해 새로운 프로미스를 반환한다.위 예제의 경우 1, 2, 3

all 메서드는 첫 번째 프로미스가 resolve 한 처리 결과부터 차례대로 배열에 저장해 그 배열을 resolve 하는 새로운 프로미스를 반환한다. 즉 **처리 순서가 보장된다.**

Promise.all 메서드의 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스도 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료된다.

```js
Promise.all([
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 1')), 3000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 2')), 2000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 3')), 1000))
])
	.then(console.log)
	.catch(console.log); // Error: Error 3
```

Promise.all 메서드의 인수로 전달받은 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스로 래핑한다.

```js
Promise.all([
	1, // Promise.resolve(1)
	2, // Promise.resolve(2)
	3, // Promise.resolve(3)
])
	.then(console.log) // [1, 2, 3]
	.catch(console.log);
```

### 45.6.3 Promise.race

Promise.race 메서드는 모든 프로미스가 fullfilled 상태가 되는것을 기다리는 것이 아니라
가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환한다.

프로미스가 rejected 상태가 되면 Promise.all 과 동일하게 처리된다.
즉 하나라도 rejected 상태가 되면 에러를 즉시 반환한다.

```js
Promise.race([
	new Promise(res => setTimeout(() => res(1), 3000)), // 1
	new Promise(res => setTimeout(() => res(2), 2000)), // 2
	new Promise(res => setTimeout(() => res(3), 1000)), // 3
])
	.then(res => console.log) // 3

Promise.race([
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 1')), 3000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 2')), 2000)),
	new Promise((_, reject) => setTimeout(() => reject(new Error('Error 3')), 1000))
])
	.then(console.log)
	.catch(console.log); // Error: Error 3
```

### 45.6.4 Promise.allSettled

Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 받는다.
전달받은 프로미스가 모두 settled 상태 (비동기 처리가 수행된 상태, 즉 fulfilled 또는 rejected 상태)가 되면 처리 결과를 배열로 반환한다.

```js
Promise.allSettled([
	new Promise(res => setTimeout(() => res(1), 2000)),
	new Promise((_, rej) => setTimeout(() => rej(new Error('Error!')), 1000))
]).then(console.log)
/*
[
	{status: 'fulfilled', value: 1},
	{status: 'rejected', reason: Error: Error! at <anonymous>:3:54 },
]
*/
```

fulfilled 또는 rejected 상태와 상관없이 처리 결과가 모두 담겨있다.
프로미스 처리결과를 나타내는 객체는 다음과 같다.
- fulfilled 상태인 경우
    - status : 비동기 처리 상태를 나타냄.
    - value : 처리 결과를 나타냄
- rejected 상태인 경우
    - status : 비동기 처리 상태를 나타냄.
    - reason : 에러를 나타냄


# 45.7 마이크로태스크 큐

어떤 로그가 출력될지 예상해보자.

```js
setTimeout(() => console.log(1), 0);

Promise.resolve()
	.then(() => console.log(2))
	.then(() => console.log(3))
```

프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 -> 2 -> 3 순으로 출력될 것 같지만
2 -> 3 -> 1 순으로 출력된다.

#태스크큐 #마이크로태스트큐
그 이유는 **프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스트 큐에 저장되기 때문이다.**

마이크로태스크 큐는 태스크 큐와 별도의 큐다.
마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장된다.
그 외의 비동기 함수의 콜백이나 이벤트 핸들러는 태스크 큐에 일시 저장된다.

콜백 함수나 이벤트 핸들러를 일시 저장한다는 점에서 태스크 큐와 동일하지만
**마이크로태스크 큐는 태스크 큐보다 우선순위가 높다.**

즉 이벤트 루프는 콜 스택이 비면 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.
이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행한다.

# 45.8 fetch

fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API다. XMLHttpRequest 객체보다 사용법이 간단하다.

fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

```text
const promise = fetch(url [, options])
```

만약 MIME 타입이 application/json 인 HTTP 응답 몸체를 취득하려면 Response.prototype.json 메서드를 사용한다. Response.prototype.json 메서드는 Response 객체에서 HTTP 응답 몸체를 취득하여 역질렬화한다.

```js
fetch('https://~').then(res => res.json()).then(json => console.log(json))
```

fetch 함수를 사용할 때에는 에러 처리에 주의해야 한다.

```js
// 부적절한 URL이 지정되어 404 Not Found 에러가 발생
fetch(wrongUrl)
	.then(() => console.log('ok'))
	.catch(() => console.log('error'))
```

catch 메서드에 의해 'error' 가 출력될 것 같지만 'ok' 가 출력된다.

**fetch 함수가 반환하는 프로미스는 기본적으로 404 Not Found 나 500 Internal Server Error와 같은
HTTP 에러가 발생해도 에러를 reject 하지 않고 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 reeslove 한다.
오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 한다.**

때문의 ok 상태를 확인해 명시적으로 에러 처리를 해야한다.

```js
fetch(wrongUrl)
	.then(res => {
		if(!res.ok) throw new Error(res.statusText);
		return res.json
	})
	.then(todo => console.log(todo))
	.catch(err => console.log(err))
```

참고로 axios 는 모든 HTTP 에러를 reject 하는 프로미스를 반환한다.
따라서 모든 에러를 catch에서 처리할 수 있어 편리하다.
또한 axios는 인터셉터, 요청 설정 등 fetch보다 더 다양한 기능을 지원한다.

fetch 함수를 통해 HTTP 요청을 전송해보자. fetch 함수에 첫 번째 인수로 HTTP 요청을 전송할 URL과 두 번째 인수로 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드등 설정한 객체를 전달한다.

```js
const request = {
	get(url) {
		return fetch(url);
	},
	post(url, payload) {
		return fetch(url, {
			method: 'POST',
			headers: { 'content-Type': 'application/json' },
			body: JSON.stringify(payload)
		})
	},
	patch(url, payload) {
		return fetch(url, {
			method: 'PATCH',
			headers: { 'content-Type': 'application/json'},
			body: JSON.stringify(payload)
		})
	},
	delete(url) {
		return fetch(url, { method: 'DELETE' })
	}
}
```

### 1. GET 요청

```js
request.get('https://~')
	.then(res => {
		if(!res.ok) throw new Error(response.statusText);
		return res.json()
	})
	.then(todos => console.log(todos))
	.catch(err => console.error(err));
```

### 2. POST 요청

```js
request.post('https://~', {
	uesrId: 1,
	title: 'JS'
}).then(res => {
	if(!res.ok) throw new Error(response.statusText);
	return res.json()
}).then(todos => console.log(todos))
  .catch(err => console.error(err));
```

### 3. PATCH 요청

```js
request.patch('https://~', {
	completed: true
}).then(res => {
	if(!res.ok) throw new Error(response.statusText);
	return res.json()
}).then(todos => console.log(todos))
  .catch(err => console.error(err));
```

### 4. DELETE 요청

```js
request.delete('https://~')
	.then(res => {
		if(!res.ok) throw new Error(response.statusText);
		return res.json()
	})
	.then(todos => console.log(todos))
	.catch(err => console.error(err));
```

# 알고 가기
---
- [ ] Promise.all / Promise.race /  Promise.allSettled 의 차이점에 대해 설명해주세요.
