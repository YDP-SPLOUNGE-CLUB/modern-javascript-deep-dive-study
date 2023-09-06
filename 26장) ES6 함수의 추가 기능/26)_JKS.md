# 26. ES6 함수의 추가 기능

## 26.1 함수의 구분

> ES6 이전까지 자바스크립트의 함수는 별다른 구분 없이 다양한 목적으로 사용되었다.

```javascript
var foo = function () {
    return 1;
};

foo();

new foo();

var obj = {foo: foo};
obj.foo();
```

> ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.

> ES6 이전의 모든 함수는 callable 이면서 constructor 이다.

```javascript
var obj = {
    x: 10,
    f: function () {
        return this.x
    };
}

console.log(obj.f());

var bar = obj.f;

console.log(bar()); // undefined

console.log(new obj.f()); // f {}
```

> 위 예제와 같이 객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 흔치는 않겠지만 문법상 가능하다는 것은 문제가 있다.
> 객체에 바인딩된 함수가 constructor 라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며 프로토타입 객체도 생성한다는 것을 의미하기 떄문이다.

> 이처럼 ES6 이전의 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없다.

| ES6 함수 구분 | constructor | prototype | super | arguments |
|-----------|-------------|-----------|-------|-----------|
| 일반함수      | O           | O         | X     | O         |
| 메서드       | X           | X         | O     | O         |
| 화살표 함수    | X           | X         | X     | X         |

> 일반 함수는 constructor 이지만 ES6 메서드, 화살표 함수는 non-constructor 이다.

## 26.2 메서드

> ES6 이전 사양에는 메서드에 대한 명확한 정의가 없었다. 일반적으로 메서드 객체에 바인됭 함수를 일컫는 의미로 사용되었다.

> ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.

```javascript
const obj = {
    x: 1,
    foo() { return this.x },
    bar: function () { return this.x },
}

```

> ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor 다.
> 따라서 생성자 함수로서 호출할 수 없다.

> 프로토타입 프로퍼티도 없고 생성하지도 않는다.

> 표준 빌트인 객체가 제공하는 프로토타입 메서드는 모두 non-constructor

> ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]] 를 가진다.

```javascript
const base = {
    name: 'Lee',
    sayHi() {
        return `Hi ${this.name}`
    }
}

const derived = {
    __proto__ : base,
    
    sayHi() {
        return `${super.sayHi()} zz`
    }
}
```

> ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다. ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]] 를 갖지 않기 때문이다.

## 26.3 화살표 함수

> function 키워드 대신 화살표를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.

> 내부 동작도 기존의 함수보다 간략하다. 콜백 함수 내에서 this 가 전역 객체를 가리키는 문제를 해결하기 위한 대용으로 유용하다.

### 26.3.1 화살표 함수 정의

### 함수 정의

> 함수 선언문으로 정의할 수 없고 표현식으로 정의해야 한다.

### 매개변수 선언

> 매개변수가 여러 개인 경우 소괄호 안에 매개변수를 선언한다.

```javascript
const arrow = (x, y) => { ... };
```

### 함수 몸체 정의
> 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략 가능하다.

> 함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호를 생략할 수 없다.

### 26.3.2 화살표 함수와 일반 함수 차이

1. 화살표 함수는 인스턴스를 생성할 수 없다.
2. 중복된 매개변수 이름을 선언할 수 없다.
3. this, argument, super, new.target 바인딩을 갖지 않는다.

### 26.3.3 this

> 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 this 다.
> 화살표 함수의 this 는 일반 함수와는 다르게 동작한다.

> 콜백 함수 내부의 this 문제, 즉 콜백 함수 내부의 this 가 외부 함수의 this 와 다르기 떄문에 발생하는 문제를 해결하기 위해 의도적으로 설계되었다.

> this 바인딩은 함수의 호출 방식, 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

```javascript
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix
    }
    
    add(arr) {
        return arr.map(function (item) {
            return this.prefix + item
        });
    }
}
```

> 프로토타입 메서드 내부인 return 메서드에서 호출한 객를 가리킨다.

> 일반 함수로서 호출되는 모든 함수 내부에는 this 는 전역 객체를 가리킨다.
> 클래스 내부에는 모든 코드에 strict mode 가 암묵적으로 적용된다.

> 이떄 발생하는 문제가 콜백 함수 내부의 this 문제이다.

> ES6 이전에는 다음과 같은 방법을 사용했다.

1. add 메서드를 호출한 prefixer 객체를 가리키는 this 를 회피시킨 후 콜백 함수 내부에서 사용한다.
2. Array.prototype.map 두 번쨰 인수로 this 를 전달
3. Function.prototype.bind 사용

> 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this 를 참조하면 상위 스코프의 this 를 그대로 참조한다. 이를 렉시컬 this 라고 한다.

> 화살표 함수를 제외한 모든 함수에는 this 바인딩이 반드시 존재한다. ES6 화살표 함수가 도입되기 이전에는 일반적인 식별자 처럼 스코프 체인을 통해 this 를 탐색할 필요가 없었다.

> 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 떄문에 Function.prototype.call , apply, bind 메서드를 사용해도 교체가 불가능하다.

### 26.3.4 super

> 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. 화살표 함수 내에서 super를 참조하면 this 와 마찬가지로 상위 스코프 super 를 참조한다.

```javascript
class Base {
    constructor(name) {
        this.name = name;
    }
    
    sayHi() {
        return `Hi ${this.name}`
    }
}

class Derived extends Base {
    sayHi = () => `${super.sayHi()} zzz`
}
```

> 내부 슬롯 [[HomeObject]] 를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드다.
> sayHi 클래스 필드에 할당한 화살표 함수는 super 바인딩을 가지지 않으므로 super 를 참조해도 에러가 발생하지 않고
> constructor super 를 참조한다. 

### 26.3.5 arguments

> 화살표 함수는 함수 자체의 argument 바인딩을 갖지 않는다. 함수 내부에서 argument 를 참조하면 this 와 마찬가지로 상위 스코프의 arguments 를 참조한다.

```javascript
(function () {
    const foo = () => console.log(arguments);
    foo(3,4);
})(1,2);

// 화살표 함수 foo arguments 는 전역 arugumetns 를 가리킨다.
const foo = () => console.log(arguments);
foo(1,2); // ReferenceError
```

> 화살표 함수로 가변 인자 함수를 구현해야 할 때는 Rest 파라미터를 사용해야 한다.

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

> Rest 파라미터는 매개변수 이름 앞에 세개의 점(...) 정의한 매개변수를 의미한다.

```javascript
function foo(...rest) {
    console.log(rest); // [1,2,3,4,5]
}

foo(1,2,3,4,5);
```

> REST 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한
> 나머지 인수들로 구성된 배열이 할당된다.

> 하나만 선언한 수 있다.

> 함수 정의 시 선언한 매개변수 개수를 나타내느 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

### 26.4.2 Rest 파라미터와 argument 객체

> ES5 에서는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수의 경우
> 매개변수를 통해 인수를 전달받는 것이 불가능하므로 arguments 객체를 활용하여 인수를 전달받았다..

> arguments 객체는 배열이 아닌 유사 배열 객체이므로 배열로 변환하여 사용하는 번거로움이 존재한다.

> rest 파라미터는 배열로 직접 전달받을 수 있으며, 화살표 함수에서 사용 가능하다.

## 26.5 매개변수 기본값

> 함수를 호출할 때 매개변수의 개수만큼 인수를 전달하는 것이 바람직하지만 그렇지 않을 경우에도 에러가 발생하지 않는다.

```javascript
function sum(x,y) {
    return x + y;
}

console.log(sum(1)); // NaN
```

> ES6 매개변수 기본값을 사용하면 인수 체크 및 초기화를 간소화 가능하다.

```javascript
function sum(x = 0, y = 0) {
    return x + y;
}

console.log(sum(1,2)); // 3
console.log(sum(1)); // 1
```
