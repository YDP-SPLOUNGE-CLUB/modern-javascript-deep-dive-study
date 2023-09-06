# 22. this

## 21. this 키워드

> 객체는 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 하나의 논리적인 단위로 묶은 복합적인 자료구조다.

> 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 프로퍼티를 참조하고 변경할 수 있어야 한다.
> 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

> 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```javascript
const circle = {
    radius: 5,

    getDiameter() {
        // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
        // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
        return 2 * circle.radius
    }
}
```

```javascript
function Circle(radius) {
????.
    radius = radius;
}

Circle.prototype.getDiameter = function () {
    return 2 * ?? ??
.
    radius
}

const circle = new Circle(5);
```

> 생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.
> new 연산자와 함께 생성자 함수를 호출하는 단계가 추가로 필요하다. 다시 말해 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 한다.

> 인스턴스를 생성하기 이전에 인스턴스를 가리키는 식별자를 알 수 없다. 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는
> 특별한 식별자가 필요하다 자바스크립트는 this 라는 특수한 식별자를 제공한다.

> this 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. this 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

> 자바스크립트 엔진에 의해 암묵적으로 생성되며 코드 어디서든 참조할 수 있다. 함수를 호출하면 arguments 객체와 this 가 암묵적으로 함수 내부에 전달된다.
> this 가 가리키는 값 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

### this 바인딩

> 바인딩이란 식별자와 값을 연결하는 과정을 의미한다.

```javascript
const circle = {
    radius: 5,
    getDiameter() {
        return 2 * this.radius;
    }
}
```

> 객체 리터럴의 메서드 내부에서의 this 메서드를 호출한 객체 circle 을 가리킨다.

```javascript
function Circle(radius) {
    // this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
}

Circle.prototype.getDiameter = function () {
    // this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius
}

const circle = new Circle(5);
```

> 자바스크립트 this 는 함수가 호출되는 방식에 따라 this 에 바인딩될 값, this 바인딩이 동적으로 결정된다.

> this 는 코드 어디에서든 참조 가능하다. 전역에서도 함수 내부에서도 참조할 수 있다.

```javascript
console.log(this); // window

function square(number) {
    // 일반 함수 내부에서 this 는 전역 객체 window 를 가리킨다.
    console.log(this);
    return number * number;
}

const person = {
    name: 'Lee',
    getName() {
        // 메서드 내부에서는 this 는 메서드를 호출한 객체를 나타낸다.
        console.log(this); // {name: 'Lee', getName: f }
        return this.name;
    }
}

function Person(name) {
    this.name = name;
    // 생성자 함수 내부에서는 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this); // Peson {name: 'Lee'}
}
```

## 22.2 함수 호출 방식과 this 바인딩

> this 바인딩은 함수 호출 방식, 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype apply call bind 메서드에 의한 간접 호출

```javascript
const foo = function () {
    console.dir(this);
}

foo(); // window;

const obj = {foo};
obj.foo(); // obj

new foo(); // foo {}

const bar = {name: 'bar'};
foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 22.2.1 일반 함수 호출

> 기본적으로 this 에는 전역 객체가 바인딩된다.

> 전역 함수는 물론이고 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

> 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 내부에는 전역 객체가 바인딩된다.

```javascript

const value = 1;

const obj = {
    value: 100,
    foo() {
        console.log(this); // {value: 100, foo: f }


        function bar() {
            console.log(this); // window;
            console.log(value); // 1
        }
    }
}
```

> 화살표 함수는 상위 스코프의 this 를 가리킨다.

```javascript
const value = 1;

const obj = {
    value: 100,
    foo() {
        setTimeout(() => console.log(this.value), 100); // 100
    }
}
```

### 22.2.2 메서드 호출

> 메서드 내부의 this 에는 메서드를 호출한 객체 즉 메서드를 호출할 때 메서드 이름 앞에 마침표 연산자 앞에 기술한 객체가 바인딩된다.

> 메서드 내부의 this 는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

```javascript
const person = {
    name: 'Lee',

    getName() {
        // this 는 메서드를 호출한 객체에 바인딩된다.
        return this.name;
    }
}
```

> getName 메서드는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다.

### 22.2.3 생성자 함수 호출

> 생성자 함수 내부 this 에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```javascript
function Circle(radius) {
    // 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    }
}

const circle = new Circle(5);
const circle2 = new Circle(10);

// 일반적인 함수는 반환문이 없으므로 undefined 를 반환
const circle3 = Circle(15);

// 일반 함수로 호출된 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 22.2.4 Function.prototype.apply / call/ bind 메서드를 통한 간접호출

> apply, call, bind 메서드는 this 로 사용할 객체와 인수 리스트를 인수로 받아 호출한다.

```javascript
function getThisBinding() {
    return this;
};

const thisArg = {a: 1};

console.log(getThisBinding());

console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
```

> apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달한다. call 메서드는 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달한다.

> apply 와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 this 로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일하다.

| 함수 호출 방식                                         | this 바인딩                            |
|--------------------------------------------------|-------------------------------------|
| 일반 함수 호출                                         | 전역 객체                               |
| 메서드 호출                                           | 메서드를 호출한 객체                         |
| 생성자 함수 호출                                        | 생성자 함수가(미래에)생성할 인스턴스                |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |

---

- [ ] this 란?
- [ ] 메서드, 생성자 함수 내에서 this 를 호출하면 어떤 일이 일어나는지 설명하시오.


