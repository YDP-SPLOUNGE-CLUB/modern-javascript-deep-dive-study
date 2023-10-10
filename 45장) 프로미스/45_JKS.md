# 45. 프로미스

> 자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다.
> 하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처리가 곤란하며
> 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있다.

## 45.1 비동기 처리를 위한 콜백 패턴의 단점

### 45.1.1 콜백 헬

> get 함수는 비동기 함수다. 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고
> 즉시 종료된다. 즉 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.
> 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면
> 기대한 대로 동작하지 않는다.

> GET 요청을 전송하고 서버의 응답을 전달받는 get 함수도 비동기 함수다. get 함수가 비동기 함수인 이유는
> get 함수 내부의 onload 이벤트 핸들러가 비동기로 동작하기 때문이다.

> get 함수를 호출하면 GET 요청을 전송하고 onload 이벤트 핸들러를 등록한 다음 undefined 를 반환하고 즉시 종료된다.
> 즉 비동기 함수인 get 함수 내부의 onload 이벤트 핸들러는 get 함수 이후에 실행된다.

> 따라서 get 함수의 onload 이벤트 핸들러에서 서버의 응답 결과를 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

```javascript
const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();
    
    xhr.onload = () => {
        if (xhr.status === 200) {
            return JSON.parse(xhr.response);
        }
    }
}

const response = get('https://jsonplcaeholder.typicode.com/posts/1');
console.log(response);
```

> get 함수가 호출되면 XMLHttpRequest 객체를 생성하고 HTTP 요청을 초기화한 후 HTTP 요청을 전송한다.
> xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하고 종료한다.

> 이떄 get 함수에 명시적인 반환문이 없으므로 get 함수는 undefined 를 반환한다.

> xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러의 반환문은 get 함수의 반환문이 아니다.
> get 함수는 반환문이 생략되었으므로 undefined 를 반환한다.

> onload 이벤트 핸들러의 반환값은 캐치할 수 없다.

> xhr.onload 핸들러 프로파티에 바인딩한 이벤트 핸들러가 즉시 실행되지 않는다.
> xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 일단 테스크 큐에 저장되어 대기하고 이벤트 루프에 의해 콜 스텍으로 푸시되어 실행된다.

> 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고 상위 스코프의 변수에 할당할 수도 없다.
> 따라서 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다.

> 이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다.
> 필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다.

> 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면
> 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데 이를 **콜백 헬** 이라 한다.

### 45.1.2 에러 처리의 한계

> 비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것이다.

```javascript
try {
    setTimeout(() => { throw new Error('Error')}, 1000);
} catch (e) {
    console.error('캐치 에러', e);
}
```

> try 코드블록 내에서 호출한 setTimeout 함수는 1초 뒤에 콜백 함수가 실행되도록 타이머를 설정하고
> 콜백 함수는 에러를 발생시킨다. 하지만 이 에러는 catch 코드 블록 내에서 캐치되지 않는다.

> 비동기 함수인 setTimeout 호출되면 실행 컨텍스트가 실행되어 콜 스택에 푸시되어 실행된다.
> setTimeout 은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거된다.

> 타이머가 만료되면 콜백 함수는 테스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.

> 에러는 호출자 방향으로 전파된다. 콜 스택의 아래 방향으로 전판된다. 하지만 setTimeout의 콜백 함수를 호출한 것은 setTimeout 이 아니다.

> setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블록에서 캐치하지 않는다.

## 45.2 프로미스의 생성

> Promise 생성자 함수를 new 연산자와 함께 호출하면 promise 객체가 생성된다.

> Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 resolve 와 reject 함수를 인수로 받는다.

```javascript
const promise = new Promise((resolve, reject) => {
    if (true) {
        resolve('result');
    } else {
        reject('fail');
    }
});
```

> Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행하고 성공시 resolve 함수를 호출하고 비동기 처리가 실패하면
> reject 함수를 호출한다.

> 프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.

- pending
  - 비동기 처리가 아직 수행되지 않은 상태
  - 프로미스가 생성된 직후 기본 상태

- fulfilled
  - 비동기 처리가 수행된 상태 (성공)
  - resolve 함수 호출

- rejected
  - 비동기 처리가 수행된 상태 (실패)
  - reject 함수 호출

> fulfilled 또는 rejected 상태를 settled 상태라 한다.

> 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.

## 45.3 프로미스의 후속 처리 메서드

> 프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 한다.
> fulfilled 상태와 rejected 상태에 따른 처리 결과를 가지고 특정 행위를 해야한다.

> 프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.

### 45.3.1 Promise.prototype.then

> then 메서드는 두 개의 콜백 함수를 인수로 전달받는다.

1. 첫 번쨰 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출된다. 이때 콜백 함수의 프로미스의 비동기 처리 결과를 인수로 받는다.
2. 두 번쨰 콜백 함수는 프로미스가 rejected 상태일 때 콜백 함수 프로미스의 에러를 인수로 받는다.

### 45.3.2 Promise.prototype.catch

> catch 메서드는 한 개의 콜백 함수를 인수로 받는다 catch 메서드는 프로미스가 rejected 상태일 경우만 호출된다.

### 45.3.3 Promise.prototype.finally

> finally 메서드는 한 개의 콜백 함수를 인수로 전달받는다. finally 메서드의 콜백 함수는 프로미스의 성공 또는 실패와 상관없이 무조건 한 번만 호출된다.

> finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다. finally 메서드도 then/catch 메서드와 마찬가지로
> 언제나 프로미스를 반환한다.

## 45.4 프로미스의 에러 처리

> 비동기 처리를 위한 콜백 패턴은 에러 처리가 곤란하다. 프로미스는 에러를 문제없이 처리 가능하다.

```javascript
promiseGet('url')
    .then(res => console.log(res))
    .catch(err => console.error(err));
```

> then 메서드에 두 번쨰 콜백 함수를 전달하지 않고 catch 메서드를 사용하여 가독성 좋고 명확하게 표현하는 것을 권장한다.

## 45.5 프로미스 체이닝

```javascript
const url = 'https://jsonplcaeholder.typicode.com/'

promiseGet(`${url}/posts/1`)
    .then(({userId}) => promiseGet(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.error(err))
```

> 위 예제에서 then -> then -> catch 순서로 후속 처리 메서드를 호출했다.
> then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다.

> 45.3 프로미스의 후속 처리 메서드에서 살펴보았듯이 후속 처리 메서드의 콜백 함수는 프로미스의 비동기 처리 상태가 변경되면 선택적으로 호출된다.

## 45.6 프로미스의 정적 메서드

> Promise 는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있다. Promise는 5가지 정적 메서드를 제공한다.

### 45.6.1 Promise.resolve / Promise.reject

> Promise.resolve / reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.

```javascript
const resolvePromse = Promise.resolve([1,2,3]);
resolvePromse.then(console.log); // [1,2,3]
```

> reject 또한 동일하다.

### 45.6.2 Promise.all

> Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

```javascript
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 3000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 3000));

const res = [];
requestData1()
    .then(data => {
        res.push(data);
        return requestData2();
    })
    .then(data => {
        res.push(data);
        return requestData3();
    })
    .then(data => {
        res.push(data);
        console.log(res);
    })
    .catch(console.error);
```
> 위 예제는 세 개의 비동기 함수 처리를 순차적으로 처리하기 때문에 총 9초 이상 소요된다.
> 위 예제의 경우 세개의 비동기 처리를 순차적으로 처리할 필요가 없다.

> Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.

```javascript
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1), 3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2), 3000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3), 3000));

Promise.all(requestData1(), requestData2(), requestData3())
    .then(console.log)
    .catch(console.error)
```

> Promise.all 메서드는 인수로 전달받은 배열의 모든 프로미스가 모두 fulfilled 상태가 되면 종료한다.

> 따라서 약 3초 정도 후에 종료된다.

> 만약 첫 번째 프로미스가 가장 나중에 fulfilled 상태가 되어도 처리 순서를 보장해준다.

### 45.6.3 Promise.race

> Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블 인수를 전달받는다.
> race 메서드는 모든 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 먼저 fulfilled 된 프로미스 처리 결과를 resolve 하는 새로운 프로미스를 반환한다.

> 프로미스가 rejected 상태가 되면 Promise.all 메서드와 동일하게 reject 프로미스를 즉시 반환한다.

### 45.6.4 Promise.allSettled

> Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
> 그리고 전달받은 프로미스가 모두 settled 상태가 되면 처리 결과를 배열로 반환한다.

> fulfilled 또는 rejected 상태와 상관없이 모든 결과가 담겨 있다.

1. 프로미스가 fulfilled 상태인 경구 비동기 처리 상태를 나타내는 status 프로피티와 처리 결과를 나타내는 value 프로퍼티를 갖는다.
2. 프로미스가 rejected 상태인 경우 비동기 처리 상태를 나타내는 status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 갖는다.

```javascript
[
    {status: "fulfilled", value: 1},
    {status: "rejected", reason: Error: Error! at <anoymous>3:~~},
]
```

## 45.7 마이크로태스크 큐

```javascript
setTimeout(() => console.log(1), 0);

Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3))
```

> 프로미스 후속 처리 메서드도 비동기로 동작하므로 1 -> 2 -> 3 으로 동작할 것 처럼 보이지만 2 -> 3 -> 1 순으로 출력한다.

> 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 마이크로태스크 큐에 저장되기 때문이다.

> 마이크로태스크 큐는 테스크 큐와는 별도의 큐다. 마이크로테스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 임시 저장된다.
> 그 외의 비동기 함수와 콜백 함수나 이벤트 핸들러는 태스크 큐에 임시 저장된다.

> 콜백 함수나 이벤트 핸들러를 임시 저장한다는 점에서 동일하지만 마이크로태스크 큐는 테스크 큐보다 우선 순위가 높다.

## 45.8 fetch

> fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API 이다.

> fetch 함수는 XMLHttpRequest 객체보다 사용법이 간단하고 프로미스를 지원하기 떄문에
> 비동기 처리를 하기 위한 콜백 패턴의 단점에서 자유롭다.

> fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환한다.

```javascript
fetch('https://jsonplcaeholder.typicode.com/todos/1')
    .then(response => console.log(response));
```

> fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 프로미스를 반환하므로 후속 처리 메서드 then 을 통해 프로미스가 resolve 한 Response 객체를 받을 수 있다.
> Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공한다.

> MIME 타입을 역직렬화 하여 취득 가능하다.

> fetch 함수를 사용할 때는 에러 처리에 주의해야 한다.

```javascript
const wrongUrl = 'https://jsonplcaeholder.typicode.com/xxx/1';

fetch(wrongUrl)
    .then(() => console.log('ok'))
    .catch(() => console.log('error'))
```

> 부적절한 URL 이 지정되었기 때문에 404 NOT FOUND 에러가 발생한다. catch 후속 처리 메서드에 의해 error 가 출력될 것처럼 보이지만 ok 가 출력된다.

> fetch 함수가 반환하는 프로미스는 기본적으로 404 / 500 같은 HTTP 에러가 발생해도 에러를 reject 하지 않고 불리언 타입의 ok 상태를 false 로 설정한
> Response 객체를 resolve 한다. 오프라인 등의 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 한다.

---

- [ ] 마이크로태스크 큐에 관해서 설명
- [ ] Promise 를 사용하였을 경우의 장점은?

