# 33. 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

> 심벌은 ES6 에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.
> 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

## 33.2 심벌 값의 생성

### 33.2.1 Symbol 함수

> 심벌 값은 Symbol 함수를 호출하여 생성한다. 다른 원시값, 즉 문자열 숫자 불리언 undefined, null 타입의 값은 리터럴 표기법을 통해
> 값을 생성할 수 있지만 심벌 값은 Symbol 함수를 호출하여 생성해야 한다.
> 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며 다른 값과 절대 중복되지않는 유일무이한 값이다.

```javascript
const mySymbol = Symbol();

console.log(mySymbol); // symbol
```

> 언뜻 보면 생성자 함수로 객체를 생성하는 것처럼 보이지만 Symbol 함수는 String, Number, Boolean 생성자 함수와는 다르게
> new 연산자와 함께 호출하지 않는다.

> Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다.
> 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용된다.

```javascript
const mySymbol1 = Symbol('1');
const mySymbol2 = Symbol('1');

console.log(mySymbol1 === mySymbol2); // false
```

### 33.2.2 Symbol.for / Symbol.ketFor 메서드

> Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

> 검색에 실패하면 새로운 심벌 값을 생성하여 메서드의 인수로 전달된 키로 전역 레지스트리에 저장한 후 생성된 심벌 값을 반환한다.

> Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출 가능하다.

```javascript
const s1 = Symbol.for('mySymbol');
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
Symbol.keyFor(s2); // undefined
```

## 33.3 심벌과 상수

```javascript
const Direction = {
    UP: 1,
    DOWN: 2,
    LEFT: 3,
    RIGHT: 4,
}

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
    console.log('You are going UP');
}
```

> 위 예제와 같은 값은 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다.
> 상수 값이 변경될 수 있으며 다른 변수 값과 중복될 수 있다는 문제점이 있다.

> 이러한 경우 변경/ 중복될 가능성이 있는 무의미한 상수 대신 중복 가능성이 없는 심벌 값을 사용할 수 있다.

```javascript
const Direction = {
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right'),
}

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
    console.log('You are going UP');
}
```

타입 스크립트의 enum 을 활용하면 심벌을 사용할 이유가 없다.

## 33.4 심벌과 프로퍼티 키

> 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며 동적으로 생성 가능하다.
> 심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.

```javascript
const obj = {
    [Symbol.for('mySymbol')]: 1
}

obj[Symbol.for('mySymbol')]; // 1
```

## 33.5 심벌과 프로퍼티 은닉

> 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ... in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.

```javascript
const obj = {
    [Symbol.for('mySymbol')]: 1
}

for (const key in obj) {
    console.log(key); // 아무것도 출력되지 않음
}

console.log(Object.getOwnPropertySymbols(obj)); // [Symobl(mysymbol)]
```

## 33.6 심벌과 표준 빌트인 객체 확장

> 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 일기 전용으로 사용하는 것이 좋다.

> 개발자가 직접 추가한 메서드와 미래의 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 떄문이다.

> 중복될 가능성이 없는 심벌 값으로 프러파티 키를 생성하여 표준 빌트인 객체를 확장하면 기존 프로퍼티 키와 충돌하지 않는 것은 물론 표준 사양의 버전이 올라감에 따라
> 추가될지 모르는 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체를 확장할 수 있다.

## 33.7 Well-known Symbol

> 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.
> Well-known Symbol 이라 부른다. 자바스크립트 엔진의 내부 알고리즘에 사용된다.

> 예를 들어 Array, String Map Set TypedArray arguments NodeList HTMLCollection 과 같이 for ... of 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol 인 Symbol.iterator 를 
> 키로 갖는 메서드를 가지며 Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 규정되어 있다.

> 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.
> ECMAScript 사양에 규정되어 있는 대로 Well-known Symbol 인 Symbol.iterator 를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.

```javascript
const iterable = {
    [Symbol.iterator]() {
        let cur = 1;
        const max = 5;
        
        return {
            next() {
                return { value: cur++, done: cur > max + 1 }
            }
        }
    }
}

for (const num of iterable) {
    console.log(num); // 1 2 3 4 5
}
```