# 10. 객체 리터럴

## 10.1 객체란?

> 자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 "모든 것"이 객체다.

> 원시 값을 제외한 나머지 값은 모두 객체다.

> 원시 타입은 단 하나의 값만 나타내지만 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이다.

> **또한 원시 타입의 값, 즉 원시 값은 변경 불가능한 값이지만 객체의 값, 즉 객체는 변경 가능한 값이다.**

> 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼타 값이 될 수 있다.
> 자바스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있다.
> 프러퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드 라 부른다.

> 객체는 객체의 상태를 나타내는 값과 프로퍼티를 참조하고 조작할 수 있는 메서드를 모두 포함할 수 있기 때문에 상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.

### 객체와 함수

> 자바스크립트의 객체는 함수와 밀접한 관계를 가진다. 함수로 객체를 생성하기도 하며
> 함수 자체가 객체이기도 하다. 자바스크립트에서 함수와 객체는 분리해서 생각할 수 없는 개념이다.

---

## 10.2 객체 리터럴에 의한 객체 생성

> C++ 나 자바 같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.

> 자바스크립트는 프로토 타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방식을 지원한다.

- 객체 리터럴
- Object 생성 함수
- 생성자 함수
- Object.create 메서드
- 클래스 (ES6)

```javascript
var person = {
    name: 'Lee',
    sayHello: function () {
        console.log(`Hello my name is ${this.name}`)
    }
}

console.log(typeof person); // object
console.log(person) // {name: "Lee", sayHello: f}
```

> 객체 리터럴의 중괄호는 코드 블록을 의미하지 않는다.

> 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프러퍼티를 만들 수도 있고
> 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.

## 10.3 프로퍼티

> 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.

> 프로퍼티를 나열할 때는 쉼표로 구분한다. 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 좋다.

> 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 한다.

> 식별자 네이밍 규칙을 따르지 않는 이상 key 값에는 반드시 따음표를 사용해야 한다.

```javascript
var person = {
	firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
	'last-name': 'Lee' // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
}

var person = {
	firstName: 'Ung-mo',
	// - 연산자가 있는 표현식으로 해석된다.
	last-name: 'Lee' // SyntaxError: Unexpected token -
}
```

> 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

## 10.4 메서드

> 함수는 값으로 취급할 수 있기 떄문에 프로퍼티 값으로 사용할 수 있다.

## 10.5 프로퍼티 접근

> 프로퍼티에 접근하는 방법은 두 가지다.

1. 마침표 프로퍼티 접근 연산자를 사용하는 마침표 표기법
2. 대괄호 프로퍼티 접근 연산자를 사용하는 대괄호 표기법


```javascript
var person = {
	name: 'Lee'
}
console.log(person.name) // Lee
console.log(person['name']) // Lee
console.log(person[name]) // ReferenceError: name is not defined
console.log(person.age) // undefined
```

> 대괄호 표기법을 사용하는 경우 대괄호 프로퍼티 접근 연산재 내부에 지정하는
> 프로퍼티 키는 반드시 따음표로 감싼 문자열 이어야 한다.

## 10.6 프로퍼티 값 갤신

> 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```javascript
var person = {
    name: 'Lee'
};

person.name = "SSS";

console.log(person); // {name: "SSS"}
```

## 10.7 프로퍼티 값 동적 생성

> 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로파티 값이 할당된다.

```javascript
var person = {
    name: 'Lee'
};

person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 10.8 프로퍼티 삭제

> delete 연산자는 객체의 프로퍼티를 삭제한다. 이때 delete 연산자의 피연산자는
> 프로퍼티 값에 접근할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티를 삭제하면
> 아무런 에러 없이 무시된다.

```javascript
var person = {
    name: 'Lee'
};

person.age = 20;
delete person.name;

delete person.something;

console.log(person); // {age: 20}
```

## 10.9 ES6 에서 추가된 객체 리터럴의 확장 기능

> ES6 에서는 더욱 간단하고 표현력 있는 객체 리터럴의 확장 기능을 제공한다.

### 10.9.1 프로퍼티 축약 표현

> 객체 리터럴의 프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성된다.
> 프로퍼티 값은 변수에 할당된 값, 식별자 표현식일 수도 있다.

```javascript
const x = 1, y= 2;

const obj = {x, y};

console.log(obj); // {x: 1, y: 2}
```

### 10.9.2 계산된 프로퍼티 이름

> 문자열 또는 문자열로 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

> 단 프로퍼티 키로 사용할 표현식을 대괄호로 묶어야 한다.


```javascript

// ES5
var prefix = 'prop';
var i = 0;

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1:1, prop-2:2, prop-3:3}

// ES6
const obj = {
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
}
console.log(obj); // {prop-1:1, prop-2:2, prop-3:3}

```

### 10.9.3 메서드 축약 표현

> ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다.

```javascript
// ES5
var obj = {
	name: 'Lee',
	sayHi: function() {
		console.log('Hi!' + this.name);
	}
};
// ES6
var obj = {
	name: 'Lee',
	sayHi() {
		console.log('Hi!' + this.name);
	}
};
```

## 느낀 점

와 ES5 로 개발하라고하면 저는 못합니다..
