# 19. 프로토타입

> 자바스크립트는 명령형 함수형 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.

> 자바스크립트는 객체 기반의 프로그래밍 언어이며 자바스크립트를 이루고 있는 거의 모든 것이 객체다.

## 19.1 객체지향 프로그래밍

> 객체지향 프로그래밍은 프로그램 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위
> 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

> 객체지향 프로그래밍은 실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작된다.

> 실체는 특징이나 성질을 나타내는 속성을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

> 프로그램에서 사람의 이름과 주소라는 속성에만 관심이 있다고 가정할때 이처럼 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표헌하는 것을 추상화 라고한다.

> 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 한다.

> 원이라는 개념을 객체로 만들어보자. 원에는 반지름이라는 속성이 있다. 반지름을 가지고 지름, 둘레 넓이를 구할 수 있다.
> 이때 반지름은 원의 상태를 나타내는 데이터이며 지름,둘레,넓이를 구하는 것은 동작이다.

```javascript
const circle = {
    radius: 5,

    // 원의 지름: 2r
    getDiameter() {
        return 2 * this.radius;
    },
    // 원의 둘레: 2 * 파이 * r
    getPerimeter() {
        return 2 * Math.PI * this.radius;
    },
    // 원의 넓이: 파이 * r^2
    getArea() {
        return Math.PI * this.radius ** 2;
    }
}

console.log(circle);
// {radius: 5, getDiameter: f , ... f}

console.log(circle.getDiameter()); // 10
```

> 이처럼 객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각한다.
> 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다.

## 19.2 상속과 프로토타입

> 상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

> 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. 중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용하는 것이다.

```javascript
function Circle(radius) {
    this.radius = radius;
    this.getArea = function () {
        return Math.PI * this.radius ** 2;
    };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는 
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.

console.log(circle1.getArea === circle2.getArea) // false;
```

> Circle 생성자 함수가 생성하는 모든 객체는 radius 프로퍼티와 getArea 메서드를 가진다.
> radius 프로퍼티 값은 일반적으로 인스턴스마다 다르다 하지만 getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를
> 사용하므로 단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
> Circle 생성자 함수는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.

> 상속을 통해 불필요한 중복을 제거해 보자. 자바스크립트는 프로토타입을 기반으로 상속을 구현한다.

```javascript
function Circle(radius) {
    this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를 공유해서 사용할 수 있도록 프로토타입에 추가한다.
Circle.prototype.getArea = function () {
    return Math.PI * this.radius ** 2;
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea) // true;
```

> Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입 즉 상위 객체 역할을 하는
> Circle prototype 의 모든 프로퍼티와 메서드를 상속받는다.
> 상속은 코드의 재사용이란 관점에서 매우 유용하다.

## 19.3 프로토타입 객체

> 프로토타입 객체란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.
> 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가지고 내부 슬롯의 값은 프로토타입의 참조다.
> [[Prototype]] 에 저장되는 프로토타입 객체 생성 방식에 의해 결정된다.

> 모든 객체는 하나의 프로토타입을 갖는다. 모든 프로토타입은 생성자 함수와 연결되어 있다.

> [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입
> 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다. 그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고
> 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### 19.3.1 __proto__ 접근자 프로퍼티

> 모든 객체는 \__proto__\ 접근자 프로퍼티를 통해 자신의 프로토타입 내부 슬롯에 간접적으로 접근할 수 있다.

### \_\_proto\_\_ 는 접근자 프로퍼티다.

> 내부 슬롯은 프로퍼티가 아니다. 따라서 자바스크립트는 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않는다.

> \_\_proto\_\_ 접근자 프로퍼티를 통해 간접적으로 내부 슬롯에 값에 접근할 수 있다.

> 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, Get, Set 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

```javascript
const obj = {};
const parent = {x: 1};

// getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수 set __proto__ 가 호출되어 프로토타입을 교체.
obj.__proto__ = parent;

console.log(obj.x) // 1
```

### \_\_proto\_\_ 접근자 프로퍼티는 상속을 통해 사용된다.

> 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype 의 프로퍼티다.
> 모든 객체는 상속을 통해 Object.property.\_\_proto\_\_접근자 프로퍼티를 사용할 수 있다.

### \_\_proto\_\_ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

> [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해
> 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```javascript
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError
```

> 위 예제에서 parent 객체를 child 객체의 프로토타입으로 설정한 후 반대로 프로토타입을 설정했다.
> 이러한 코드가 에러 없이 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지기 떄문에
> \_\_proto\_\_ 접근자 프로퍼티는 에러를 발생시킨다.

> 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.

### \_\_proto\_\_ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

> 코드 내에서 \_\_proto\_\_ 접근자 프로퍼티를 직접 사용하는 것은 권장하지 않는다.
> 모든 객체가 \_\_proto\_\_ 접근자 프로퍼티를 사용할 수 있는것은 아니기 때문이다.

```javascript
// obj는 프로토타입 체인의 종점이다. Object.__proto__ 를 상속받을 수 없다.
const obj = Object.create(null);

console.log(obj.__proto__); // undefined

console.log(Object.getOwnPropertyDescriptor(obj)); // null
```

> 따라서 \_\_proto\_\_ 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는
> Object.getPrototypeOf 메서드를 사용하고 교체가 필요할 경우 Object.setPrototypeOf 메서드를 사용하는 것을 권장한다.

```javascript
const obj = {};
const parent = {x: 1};

Object.getPrototypeOf(obj); // obj.__proto__

Object.setPrototypeOf(obj, parent) // obj.__proto__ = parent;
```

## 19.3.2 함수 객체의 prototype 프로퍼티

> 함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

> 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며
> 프로토타입도 생성하지 않는다.

```javascript
const Person = (name) => {
    this.name = name;
}

console.log(Person.hasOwnProperty('prototype')); // false

console.log(Person.prototype); // undefined

const obj = {
    foo() {
    }
};

console.log(obj.foo.hasOwnProperty('prototype')); // false
console.log(obj.foo.prototype); // undefined
```

> 모든 객체가 가지고 있는 __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.

| 구분                 | 소유          | 값         | 사용 주체  | 사용 목적                                 |
|--------------------|-------------|-----------|--------|---------------------------------------|
| __proto__ 접근자 프로퍼티 | 모든 객체       | 프로토타입의 참조 | 모든 객체  | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용       |
| prototype 프로퍼티     | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

> 모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는
> 생성자 함수를 가리킨다. 이 연결은 생성자 함수가 생성될 때 즉 함수 객체가 생성될 때 이뤄진다.

```javascript
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');

console.log(me.constructor === Person); // true
```

> 위 예제에서 Person 생성자 함수는 me 객체를 생성했다. 이때 me 객체는 프로토타입의 constructor 프로퍼티를 통해 생성자 함수와 연결된다.
> me 객체에는 constructor 프로퍼티가 없지만 me 객체의 프로토타입은 Person.prototype 에는 constructor 프로퍼티가 존재한다.
> 따라서 me 객체는 Person.prototype 의 constructor 프로퍼티를 상속받아 사용할 수 있다.

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

```javascript
const obj = new Object();
console.log(obj.constructor === Object); // true

const add = new Function('a', 'b', 'return a + b');
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');
console.log(me.constructor === Person); // true;
```

> 리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

```javascript
const obj = {};
const add = function (a, b) { return a + b };

const arr = [1,2,3];

const regExp = /is/ig;
```

> 리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다. 하지만 프로토타입의 constructor 프로퍼티가 가리키는
> 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

> Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null 인수로 전달하면서 호출하면 내부적으로 추상 연산
> OrdinaryObjectCreate 를 호출하여 Object.prototype 을 프로토타입으로 갖는 빈 객체를 생성한다.

```javascript
let obj = new Object();
console.log(obj); // {}

class Foo extends Object {}

new Foo(); // Foo {}

obj = new Object(123);

console.log(obj) // Number {123}
```

> 이처럼 추상 연산을 호출하여 빈 객체를 생성하는 점에서 동일하나 new.target 의 확인이나 프로퍼티를 추가하는 처리 등 세부 내용은 다르다.
> 따라서 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

```javascript
function foo() {}

console.log(foo.constructor === Function); // true;
```

> 리터럴 표기법에 의해 성성된 객체도 상속을 위해 프로토타입이 필요하다. 가상적인 생성자 함수를 갖는다.

> 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

> 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한
> 생성자 함수로 생각해도 무리는 없다.

## 19.5 프로토타입의 생성 시점

> 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. 프로토타입과 생성자 함수는 단독으로 존재할 수 없고
> 언제나 쌍으로 존재하기 떄문이다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

> 생성자 함수로서 호출할 수 있는 함수 즉 constructor 는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

```javascript
console.log(Person.prototype); // {constructor: f}

function Person(name) {
    this.name = name;
}
```

> 생성자 함수로서 호출할 수 없는 함수 즉 non-constructor 는 프로토타입이 생성되지 않는다.

```javascript
const Person = (name) => {
    this.name = name;
}

console.log(Person.prototype); // undefined
```

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

> 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
> 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.

> 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 생성자 함수 또는 리터럴 표기법으로
> 객체를 생성하면 프로토타입은 생성된 객체의 Prototype 내부 슬롯에 할당된다.

