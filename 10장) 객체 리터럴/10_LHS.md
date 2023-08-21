# 10.1 객체란?

자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 모든 것이 객체다.

원시 값을 제외한 나머지 값(함수, 배열, 정규표현식 등)은 모두 객체다.

>원시 타입의 값, 즉 원시 값은 변경 불가능한 값이지만
>객체 타입의 값, 즉 객체는 변경 가능한 값이다.


객체는 0개 이상의 프로퍼티로 구성된 집합이며 프로퍼티는 키(key)와 값(value)으로 구성된다.
```js
var person = {
	name: 'Lee', // 프로퍼티
	[프로퍼티 키]: 프로퍼티 값 // 프로퍼티
}
```

자바사크립트의 함수는 일급 객체이므로 값으로 취급할 수 있다.
따라서 함수도 프로퍼티 값으로 사용할 수 있다.
프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드(method)라 부른다.

```js
var counter = {
	// property
	num: 0,
	// method
	increase: function () {
        this.num++;
	}
}
```

객체는 프로퍼티와 메서드로 구성된 집합체다.
> 프로퍼티 : 객체의 상태를 나타내는 값
> 메서드 : 프로퍼티(상태데이터를 참조하고 조작할 수 있는 동작


객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 객체지향 프로그래밍이라 한다.


# 10.2 객체 리터럴에 의한 객체 생성
C++ 이나 자바 같은 클래스 기반 객체지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스를 생성하는 방식으로 객체를 생성한다.

### 인스턴스

> 인스턴스란 클래스에 의해 생성되어 메모리에 저장된 실체를 말한다.
> 객체지향 프로그래밍에서 객체는 클래스와 인스턴스를 포함한 개념이다.
> 클래스는 인스턴스를 생성하기 위한 템플릿의 역할을 한다.
> 인스턴스는 객체가 메모리에 저장되어 실제로 존재하는 것에 초점을 맞춘 용어다.

**자바스크립트는 프로토타입 기반 객체지향 언어**로서 클래스 기반 객체지향 언어와는 달리 객체 생성방법을 지원한다.
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

객체 리터럴은 객체를 생성하기 위한 표기법이다.
객체 리터럴은 중괄호 {...} 내에 0개 이상의 프로퍼티를 정의한다.
변수에 할당되는 시점에 자바스크립트엔진은 객체 리터럴을 해석해 객체를 생성한다.

```js
var empty = {} // 빈 객체
var person = {
	name: 'Lee',
	sayHello: function () {
		console.log(`Hello! My name is ${this.name}`);
	}
};

console.log(typeof empty) // object
console.log(typeof person) // object
console.log(person) // {name: 'Lee', sayHello: f}
```

# 10.3 프로퍼티

**객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**

```js
var person = {
	name: 'Lee',
	age: 20,
	[프로퍼티 키]: [프로피티 값]
}
```

- 프로퍼티 키 : 빈 문자열을 포험하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

심벌 값도 프로퍼티 키로 사용할 수 있지만 일반적으로 문자열을 사용한다.
프로퍼티 키는 문자열이므로 '' 또는 "" 로 묶어야 한다.
자바스크립트에서 사용 가능한 유효한 이름인 경우 따옴표를 생략할 수 있다.
**식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.**

```js
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

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생설할 수도 있다.
이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호\[\]로 묶어야 한다.

```js
var obj = {};
var key = 'hello';

// ES5: 프로퍼티 키 동적 생성
obj[key] = 'world';
// EX6: 계산된 프로퍼티 이름
// var obj = { [key]: 'world' }

console.log(obj); // {hello: "world"}
```

빈 문자열을 프로퍼티 키로 사용해도 에러가 발생하지는 않는다. 하지만 키의 의미를 갖지 못하므로 권장하지 않음

```js
var foo = {
	'': ''
}
console.log(foo) // {"": ""}
```

프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 된다.
프로퍼티 키에 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환된다.

```js
var foo = {
	0: 1,
	1: 2,
	2: 3
};
console.log(foo); // {0: 1, 1: 2, 2: 3}
```

var, function과 같은 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 안흔ㄴ다.
하지만 예상치 못한 에러가 발생할 여지가 있으니 권장하지 않음

```js
var foo = {
	var: '',
	function: ''
}
console.log(foo); // {var: '', function: ''}
```

이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.
에러가 발생하지 않는다는 점에 주의하자.

```js
var foo = {
	name: 'Lee',
	name: 'Kim'
};
console.log(foo); // {name: 'Kim'}
```

# 10.4 메서드

자바스크립트의 함수는 객체(일급 객체)다.
따라서 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드(method)라 부른다.
즉, 메서드는 객체에 묶여있는 함수를 의미한다.

```js
var circle = {
	radius: 5,
	getDiameter: function () { // method
		return 2 * this.radius
	}
}
```

# 10.5 프로퍼티 접근
프로퍼티 접근하는 방법
- 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법(dot notation)
- 대괄호 프로퍼티 접근 연산자(\[\])를 사용하는 대괄호 표기법(bracket notation)

```js
var person = {
	name: 'Lee'
}
console.log(person.name) // Lee
console.log(person['name']) // Lee
console.log(person[name]) // ReferenceError: name is not defined
console.log(person.age) // undefined
```

- **대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열**이어야 한다.
- **객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.**

```js
var person = {
	'last-name': 'Lee',
	1: 10
};

person.'last-name'; // SyntaxError: Unexpected string
person.last-name;   // 브라우저 환경: NaN
					// Node.js 환경: ReferenceError: name is not defined
person.[last-name]; // ReferenceError: last is not defined
person.['last-name']; // Lee

// 프로퍼티 키가 숫자로 이루어진 문자열인 경우 따옴표를 생략할 수 있다.
person.1; // SyntaxError: Unexpected number
person.'1'; // SyntaxError: Unexpected string
person[1]; // 10 : person[1] -> person['1']
person['1']; // 10
```

Node.js 환경에서 person.last-name의 실행 결과는 ReferenceError: name is not defined 이고
브라우저 환경에서 person.last-name의 실행 결과는 NaN 이다.

실행 결과
1. person.last-name 을 샐행할 때 자바스크립트 엔진은 먼저 person.last 를 평가한다.
2. person 객체에는 프로퍼티 키가 last인 프로퍼티가 없다.
3. 때문에 person.last는 undefined로 평가된다. (person.last-name 은 undefined-name과 같다.)
5. 다음으로 name이라는 식별자를 찾는다. (name은 프로퍼티 키가 아니라 식별자로 해석된다.)
6. Node.js 환경에서는 현재 어디에도 name이라는 식별자가 없으므로 ReferenceError 가 발생한다.
7. 브라우저 환경에는 name 이라는 전역변수 (전역 객체 window의 프로퍼티)가 암묵적으로 존재한다.
    1. 전역변수 name은 창(window)의 이름을 가리키며, 기본값은 빈 문자열이다.
    2. 따라서 person.last-name은 undefined - '' 과 같으므로 NaN 이 된다.


# 10.6 프로퍼티 값 갱신
이미 존재하는 프로퍼티에 값을 할당하면 값이 갱신된다.

```js
var person = {
	name: 'Lee'
}

person.name = 'Kim'
consoe.log(person.name) // Kim
```

# 10.7 프로퍼티 동적 생성

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고
프로퍼티 값이 할당된다.

```js
var person = {
	name: 'Lee'
}
// person 객체에 age 프로퍼티가 존재하지 않는다. age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;
console.log(person); // {name: 'Lee', age: 20}
```

# 10.8 프로퍼티 삭제
delete 연산자는 객체의 프로퍼티를 삭제한다.
이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.
만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러없이 무시된다.

```js
var person = {
	name: 'Lee'
}

person.age = 20;
delete person.age;

delete person.address;

console.log(person); // {name: 'Lee'}
```

# 10.9 ES6에 추가된 객체 리터럴의 확장 기능
## 10.9.1 프로퍼티 축약 표현
객체 리터럴의 프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성된다.
프로퍼티 값은 변수에 할당된 값, 즉 식별자 표현식일 수도 있다.

```js
var x = 1, y = 2;
// ES5
var obj = {
	x: x,
	y: y,
}

// 프로퍼티 키와 변수 이름이 동일할 때 생략 할 수 있다.
// ES6
var obj = {x, y};
```

## 10.9.2 계산된 프로퍼티 이름
문자열 또는 문자열 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.

```js

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

## 10.9.3 메서드 축약 표현

```js
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

ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다.
이는 이후 메서드에 자세히 다룰 때 알아보자.

# 알고 가기
---
- [ ] 객체란 무엇인가요?
