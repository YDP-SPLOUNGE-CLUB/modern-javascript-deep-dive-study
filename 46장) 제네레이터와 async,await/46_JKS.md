# 46장 제네레이터와 async/await

## 46.1 제네레이터란?

> ES6 에서 도입된 제네레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
> 제네레이터와 일반 함수의 차이는 다음과 같다.

1. 제네레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.

일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다. 함수 호출자는 함수를 호출한 이후 함수의 실행을 제어할 수 없다.
제네레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다. 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다.

이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도할 수 있다는 것을 의미한다.

2. 제네레이터 함순느 함수 호출자와 함수의 상태를 주고받을 수 있다.

일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
즉 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다. 제너레이터 함수는 함수 호출자와 양방향으로 함수의
상태를 주고받을 수 있다. 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수 있다.

3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다. 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인
제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의

> 제너레이터 함수는 function* 키워드로 선언한다 하나 이상의 yield 표현식을 포함한다. 이것을 제외하면 일반 함수를 정의하는 방법과 같다.

```javascript
// 제너레이터 함수 선언문
function* getDecFUnc() {
    yield 1;
}

// 제너레이터 함수 표현식
const getExpFunc = function* () {
    yield 1;
}

// 제너레이터 메서드
const obj = {
    * genObjMethod() {
        
    }
}

// 제너레이터 클래스 메서드
class MyClass {
    * getClsMethod() {
        yield 1;
    }
}
```

> 에스터리스크(*) 의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다. 다음 예제의 제너레이터 함수는 모두 유효하다.

```javascript
function* getFunc() { yield 1; }
function * getFunc() { yield 1; }
function *getFunc() { yield 1; }
function*getFunc() { yield 1; }
```

> 제너레이터 함수는 화살표 함수로 정의 할 수 없다.

> 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.


## 46.3 제너레이터 객체

> 제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 거시 아니라 제너레이터 객체를 생성해 반환한다.
> 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.

> 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는
> next 메서드를 소유하는 이터레이터이다.

```javascript
function* getFunc() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = getFunc();

console.log(Symbol.iterator in generator); // true
console.log('next' in generator); // true
```

> 제너레이터 객체는 next 메서드를 갖는 이터레이터지만 이터레이터에는 없는 return , throw 메서드를 갖는다.

next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield 값을 value 프로퍼티 값으로,
false 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체로 반환한다.

return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로 true 를 done 프로파티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```javascript
function* getFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch (e) {
        console.error(e);
    }
}

const generator = getFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return('End;')); // {value: End, done: true}
```

throw 메서드르 호출하면 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로 true 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다

## 46.4 제너레이터 일시 중지와 재개

> 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
> 일반 함수는 호출 이후 제어권을 함수가 독점하지만 제너레이터는 함수 호출자에게 제어권을 양도하여 필요한 시점에 함수 실행을 재개할 수 있다.

> 제너레이터 함수를 호출하면 제너레이터 함수의 코드 블록이 실행되는 것이 아니라 제너레이터 객체를 반환한다고 했다.
> 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다. 제너레이터 객체의 next 메서드를 호출하면
> 함수의 코드 블록을 실행한다.

> 단 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행한다.
> yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```javascript
function* getFunc() {
    yield 1;
    yield 2;
    yield 3;
}

const generator = getFunc();

console.log(generator.next()); // {value:1 done:false}

console.log(generator.next()); // {value:2 done:false}

console.log(generator.next()); // {value:3 done:false}

console.log(generator.next()); // {value:undefined done:true}
```

> 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 호출자로 양도된다.

> 이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
> next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield 된 값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었느지
> 나타내는 불리언 값이 할당된다.

> 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

## 46.5 제너레이터의 활용

### 46.5.1 이터러블 구현

> 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.
> 먼저 이터레이션 프로토콜을 준수하여 무한 피보나치 수열을 생성하는 함수를 구현하자.

```javascript
const infiniteFibonacci = (function (){
    let [pre, cur] = [0, 1];
    
    return {
        [Symbol.iterator]() { return this },
        next() {
            [pre, cur] = [cur, pre + cur];
            
            return { value: cur };
        }
    }
})();

for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);
}
```

```javascript
const infiniteFibonacci = (function* (){
    let [pre, cur] = [0, 1];
    
    while (true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
})();

for (const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num);
}
```

### 46.5.2 비동기 처리

> 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
> 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구분 가능하다.

```javascript
const fetch = require('node-fetch');

const async = generatorFunc => {
    const generator = generatorFunc();
    
    const onResolved = arg => {
        const result = generator.next(arg);
        
        return result.done ? result.value : result.value.then(res => onResolved(res));
    }
    
    return onResolved;
}

(async(function *fetchTodo() {
    const url = 'https://hsonplaceholder.typicode.com/todos/1';
    
    const response = yield fetch(url);
    const todo = yield response.json();
    console.log(todo);
}))();
```

1. async 함수가 호출되면 인수로 전달받은 제너레이터 함수 fetchTodo 를 호출하여 제너레이터 객체를 생성하고
onResolved 함수를 반환한다. onResolved 함수는 상위 스코프의 generator 변수를 기억하는 클로저다. async 함수가 반환한 onResolved 함수를 즉시 호출하여 
생성한 제너레이터 객체의 next 메서드를 처음 호출한다.

2. next 메서드가 처음 호출되면 제너레이터 함수 fetchTodo 첫 번쨰 yield 문까지 실행된다. 이때 next 메서드가 반환한 이터레이터 리절트 객체의 done
프로퍼티 값이 false, 즉 아직 제너레이터 함수가 끝까지 실행되지 않았다면 이터레이터 리절트 객체의 value 프로퍼티 값, 첫 번째 yield 된 fetch 함수가 반환한
프로미스가 resolve 한 Response 객체를 onResolved 함수에 인수로 전달하면서 재귀 호출한다.

3. onResolved 함수에 인수로 전달한 Response 객체를 next 메서드에 인수로 전달하여 next 메서드를 두 번째 호출한다. 
이때 next 메서드를 두 번쨰로 호출한다. 이때 next 메서드에 인수로 전달한 Response 객체는 제너레이터 함수 fetchTodo 의 response 변수에 할당되고
제너레이터 함수 fetchTodo 의 구 번째 yield 문 까지 실행된다.

4. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 false 즉 아직 제너레이터 함수 fetchTodo 가 끝까지 실행되지 않았다면
이터레이터 리절트 객체의 value 프로퍼티 값 즉 두번쨰 yield response json 메서드가 반환한 프로미스가 resolve 한 Todo 객체를
onResolved 함수에 인수로 전달하여 재귀 호출

5. onResolved 함수에 인수로 전달한 todo 객체를 next 메서드에 인수로 전달하면서 세 번째 호출. 이때 next 인수로 전달한
todo 객체는 제너레이터 함수 fetchTodo 의 todo 변수에 할당되고 제너레이터 함수 fetchTodo 가 끝까지 실행된다.

6. next 메서드가 반환한 이터레이터 리절트 객체의 done 프로퍼티 값이 true, 즉 제너레이터 함수 fetchTodo 가 끝까지 실행되었다면
이터레이터 리절트 객체의 value 프로퍼티 값 fetchTodo 반환인 undefined 를 반환하고 처리를 종료한다.

## 46.6 async / await

> 제너레이터를 사용해서 비동기 처리를 동기 처리처럼 동작하도록 구현했지만 복잡하고 가독성도 나쁘다.
> ES8 에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async / await 가 도입되었다.

> async / await 프로미스를 기반으로 동작한다. 프로미스의 then / catch / finally 후속 처리 메서드에 콜백 함수를 전달하여 비동기 처리 결과를
> 후속 처리할 필요 없이 동기 처리처럼 프로미스를 사용할 수 있다. 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

```javascript
const fetch = require('node-fetch');

async function fetchTodo() {
    const url = 'https://hsonplaceholder.typicode.com/todos/1';
    
    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
};

fetchTodo();
```

### 46.6.1 async 함수

> await 키워드는 반드시 async 함수 내부에서 사용해야 한다. async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
> async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다.

### 46.6.2 await 키워드

> await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 처리 결과를 반환한다.
> await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

### 46.6.3 에러 처리

> 비동기 처리를 위한 콜백 패턴의 단점 중 가장 심각한 점은 에러 처리가 곤란하다는 점이다.
> async / await 에러 처리는 try ... catch 문을 사용할 수 있다.

---

- [ ] 다음 코드의 실행 결과는?
```javascript
function getFunc() {
    
    const x = yield 1;
    
    const y = yield (x + 10);
    
    return x + y;
}

const generator = getFunc();

let res = generator.next();
console.log(res);

res = generator.next(10);
console.log(res);

res = generator.next(20);
console.log(res);
```