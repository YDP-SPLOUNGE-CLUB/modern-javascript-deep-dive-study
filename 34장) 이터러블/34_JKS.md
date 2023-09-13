# 34. 이터러블

## 34.1 이터레이션 프로토콜

> 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들귀 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

> ES6 이전의 순회 가능한 데이터 컬렉션, 배열, 문자열 유사 배열 객체 DOM 컬렉션 등은 통일된 규약 없이 각자 나름의 궂를 가지고 for 문 for ... in 문
> forEach 메서드 등 다양한 방법으로 순회할 수 있었다.

> ES6 에서는 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for ... of 문 스프레드 문법, 배열 디스트럭처링 하당의 대상으로 사용할 수 있도록 일원화 했다.

1. 이터러블 프로토콜

Well-known Symbol Symbol.iterator 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해
상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
이러한 규약을 이터러블 프로토콜이라 하며 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 for ... of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수
있다.

2. 이터레이터 프로토콜

이터러블 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다.
이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value 와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.
이러한 규약을 이터레이터 프로토콜 이라 하며 이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.
이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

### 34.1.1 이터러블

> 이터러블 프로토콜을 준수한 객체를 이터러블 이라 한다. Symbol.iterator 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

> 이터러블 확인 함수는 다음과 같이 구현할 수 있다.

```javascript
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function'

isIterable([]); // true
isIterable(''); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false
```

> 예를 들어 배열은 Array.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블이다.

> 이터러블은 for ... in 문으로 순회 가능하고 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용 가능하다.

```javascript
const array = [1, 2, 3];

console.log(Symbol.iterator in array); // true

for (const item of array) {
    console.log(item)
}

console.log([...array])

const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

> Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
> 따라서 일반 객체는 for ... of 문으로 순회할 수 없으며 스프레드 문법과 배열 디스트럭처링 할당듸 대상으로 사용 불가능하다.

> 단 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용하게 되었다.

```javascript
const obj = {a: 1, b: 2};

console.log({...obj}); // {a:1, b:2}
```

### 34.1.2 이터레이터

> 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.

```javascript
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

console.log('next' in iterator); // true
```

> next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉 next 메서드를 호출하면
> 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환한다.

```javascript
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

> 자바스크립트는 이터레이션 프로토콜을 준수한객체인 빌트인 이터러블을 제공한다.
> 다음 표준 빌트인 객체들은 빌트인 이터러블이다.

| 빌트인 이터러블   | Symbol.iterator 메서드                                                           |
|------------|-------------------------------------------------------------------------------|
| Array      | Array.prototype[Symbol.iterator]                                              |
| String     | String.prototype[Symbol.iterator]                                             |
| Map        | Map.prototype[Symbol.iterator]                                                |
| Set        | Set.prototype[Symbol.iterator]                                                |
| TypedArray | TypedArray.prototype[Symbol.iterator]                                         |
| arguments  | arguments[Symbol.iterator]                                                    |
| DOM 컬렉션    | NodeList.prototype[Symbol.iterator] HTMLCollection.prototype[Symbol.iterator] |


## 34.3 for ... of 문

> for ... of 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다. for ... of 문의 문법은 다음과 같다.

**for (변수선언문 of 이터러블)**

> for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]] 의 값이 ture 인 프로퍼티를
> 순회하며 열거한다. 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.

> for ... of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터
> 리절트 객체의 value 프로퍼티 값을 for ... of 문의 변수에 할당한다.
> done 프로퍼티 값이 false 라면 순회를 계속하고 true 라면 순회를 중단한다.

## 34.4 이터러블과 유사 배열 객체

> 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.
> 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```javascript
const arrayLikes = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};

for (let i = 0; i < arrayLikes.length; i++) {
    console.log(arrayLikes[i]); // 1 2 3
}
```

> 유사 배열 객체는 이터러블이 아닌 일반 객체이므로 for ... of 문으로 순회 불가능하다.

> 단 arguments, NodeList, HTMLCollection 은 유사 배열 객체이면서 이터러블이다.
> ES6 에서 이터러블이 도입되면서 유사 배열 객체인 arguments, NodeList, HTMLCollection 객체에
> Symbol.iterator 메서드를 구현하여 이터러블이 되었다.

## 34.5 이터레이션 프로토콜의 필요성

> for ... of 문, 스프레드 문법, 배열 디스트럭처링 할당 등은 다양한 데이터 소스를 사용 가능하다.
> (Array, String ,Set, Map, TypedArray, DOM 컬렉션) 등등

> 이터러블은 for ... of 문 스프레드 문법 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로
> 데이터 공급자의 역할을 한다고 할 수 있다.

> 만약 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야 한다.
> 이는 효율적이지 않다. 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할을 하므로
> 매우 편리하게 사용 가능하다.

## 34.6 사용자 정의 이터러블

### 34.6.1 사용자 정의 이터러블 구현

```javascript
const fibonacci = {
    [Symbol.iterator]() {
        let [pre, cur] = [0, 1];
        const max = 10;
        
        return {
            next() {
                [pre, cur] = [cur, pre + cur];
                return { value: cur, done: cur >= max };
            }
        }
    }
}

for (const num of fibonacci) {
    console.log(num); // 1, 2, 3, 5, 8
}
```

### 34.6.2 이터러블을 생성하는 함수

```javascript
const fibonacci = function (max) {
    let [pre, cur] = [0, 1];
    
    return {
        [Symbol.iterator]() {
            return {
                next() {
                    [pre, cur] = [cur, pre + cur];
                    return { value: cur, done: cur >= max };
                }
            }   
        }
    }
    
}

for (const num of fibonacci(10)) {
    console.log(num); // 1, 2, 3, 5, 8
}
```

### 34.6.3 이터러블이면서 이터레이터인 객체를 생성하는 함수

```javascript
const iterable = fibonacci(5);

const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: true }
```

> 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 변경

```javascript
const fibonacci = function (max) {
    let [pre, cur] = [0, 1];
    
    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur, done: cur >= max };
        }
    }
    
}

let iter = fibonacci(10);

for (const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fibonacci(10);

console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```

### 34.6.4 무한 이터러블과 지연 평가

```javascript
const fibonacci = function () {
    let [pre, cur] = [0, 1];
    
    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur };
        }
    }
    
}
```

> 이터러블은 데이터 공급자의 역할을 한다. 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급한다.
> 하지만 위 예제 이터러블은 지연 평가를 통해 데이터를 생성한다.

> for ... of 문이나 배열 디스트럭처링 할당 등이 실행되기 이전까지 데이터를 생성하지 않는다. for ... of 문의 경우 이터러블을 순회할 때 내부에서 이터레이터의
> next 메서드를 호출하는데 바로 이때 데이터가 생성된다.

> 지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있으며 메모리를 절약할 수 있다.

---

- [ ] 이터레이터 이터러블의 차이점을 서술하시오.