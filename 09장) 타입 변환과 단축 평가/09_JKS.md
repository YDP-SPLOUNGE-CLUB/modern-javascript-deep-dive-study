# 9. 타입 변환가 단축 평가

## 9.1 타입 변환이란?

> 자바스크립트의 모든 값은 타입이 있다. 값의 타입은 개발자의 의도에 따라 다른 타입으로 변환활 수 있다.

> 개발자가 의도적으로 값의 타입을 변환하는 것을 명시적 타입 변환 또는 타입 캐스팅 이라 한다.

```javascript
var x = 10;

var str = x.toString();
console.log(typeof str, str) // string 10
```

> 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 한다.
> 이를 암묵적 타입변환 또는 타입 강제 변환 이라 한다.

> 명시적 타입 변환만 사용하고 암묵적 타입 변환은 발생하지 않도록 코드를 작성하면 어떨까?
> 좋은 생각이긴 하지만 이러한 논리는 옳지 않다. 때로는 명시적 타입 변환보다 암묵적 타입 변환이
> 가독성 측명에서 더 좋을 수 있다.

## 9.2 암묵적 타입 변환

> 자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할 때가 있다.

```javascript
'10' + 2 // 102

5 * '10' // 50
```

### 9.2.1 문자열 타입 변환

> 자바스크립트 엔진은 문자열 타입이 아닌 값을 문자열 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작한다.

```javascript
// 숫자 타입
0 + '' // "0"
-0 + '' // "0"
1 + '' // "1"
-1 + '' // "-1"
NaN + '' // "NaN"
Infinity + '' // "Infinity"
-Infinity + '' // "-Infinity"

// 불리언 타입
true + '' // "true"
false + '' // "false"

// null 타입
null + '' // "null"

// undefined 타입
undefined + '' // "undefined"

// 심벌 타입
(Symbol()) + '' // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '' // [object Object]
Math + '' // [object Object]
[] + '' // ""
[10, 20] + '' // "10,20"
(function(){}) + '' // "function(){}"
Array + '' // function Array() {[native code]}

```

### 9.2.2 숫자 타입으로 변환

```javascript
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
```

> 예제에서 사용한 연산자는 모두 산술 연산자이다.
> 자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

> 피연산자를 숫자 타입으로 변환해햐 할 문맥은 산술 연산자뿐만이 아니다.
```javascript
'1' > 0 // true
```

> 비교 연산자의 역할은 불리언 값을 만드는 것이다.
> 코드의 문맥상 모두 숫자 타입이어야 한다. 자바스크립트 엔진은 비교 연산자 표현식을 평가하기 위해
> 비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.

> 숫자 타입이 아닌 값을 숫자 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작한다.

> "+" 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

```javascript
// 문자열 타입
+'' // 0
+'0' // 0
+'1' // 1
+'string' // NaN

// 불리언 타입
+true // 1
+false // 0

// null 타입
+null // 0

// undefined 타입
+undefined // NaN

// 심벌 타입
+Symbol() // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{} // NaN
+[] // 0
+[10, 20] // NaN
+(function(){}) // NaN

```

### 9.3.3 불리언 타입으로 변환

```javascript
Boolean('x') // true
Boolean('') // false

Boolean(NaN) // false
Boolean(undefined) // false
Boolean(0) // false

Boolean(1) // true

|| 'x' // true
|| '' // false

// 비슷함
```

## 느낀점

개인적으로 암묵적 타입 변환을 그렇게 선호하지는 않습니다.

처음부터 명확하게 표시를 해서 보여주는 편이 나중에 좀 더 가독성이 좋지 않나 싶습니다.

다만 String 으로 들어오는 API 등을 Number 로 변환할 때에는 + 연산자를 사용해서 바꾸었던 기억이 있는데

어느정도로 섞어 쓰는게 좋을지. 어느 선까지 사용하면 좋을지. 생각하면 좋을 것 같아요.


## 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가

```javascript
'Cat' || 'Dog' // 'Cat'
'Cat' && 'Dog' // 'Dog'
```

> 논리합(||) 연산자는 두 개의 피연산자 중 하나만 true 로 평가되어도 true 를 반환한다.

> 논리곱(&&) 연산자는 두 개의 피연사자 중 둘 다 true 로 평가되어야 한다.

> 조건이 Falsy 값일 때 무연가를 해야 한다면 논리합 연산자 표현식으로 if 문을 대체할 수 있다.

> 객체를 가리키기를 기대하는 변수가 null 또는 undefined 가 아닌지 확인하고 프로퍼티를 참조할 때

> 객체는 키와 값으로 구성된 프로퍼티의 집합이다. 만약 객체를 가리키기를 기대하는 변수의 값이 null 또는 undefined 인 경우 객체의 프로퍼티를 참조하면
> 타입 에러가 발생한다. 에러가 발생하면 프로그램을 강제 종료한다.

> 이때 단축 평가를 새용하면 에러를 발생시키지 않는다.

```js
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // null;
```

> 함수 매개변수에 기본값을 설정할 때

> 함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이때 단축 평가를 사용해
> 매개변수의 기본값을 설정하면 undefined 로 인해 발생하는 에러를 방지할 수 있다.

```javascript
function getStringLength(str) {
	str = str || '';
	return str.length;
}

// ES6
function getStringLength(str = '') {
	return str.length;
}

getStringLength(); // 0
getStringLength('hi'); // 2
```

### 9.4.2 옵셔널 체이닝 연산자

> ES11 에서 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는
> undefined 일 경우 undefined 를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```js
var elem = null;

var value = elem?.value;
console.log(value); // undfeind
```

## 느낀 점

아 이거 많이쓰죠..

하지만 필수적으로 들어가는 값에 오류가 나지 않으려고 반창고 붙이듯이 옵셔널 체이닝을 사용한다면

오히려 독이 될 수 있다고 생각합니다.

### 9.4.3 null 병합 연산자

> null 병합 연산자 ?? 는 좌항의 피연산자가 null 또는 undefined 인 경우
> 우항의 피연산자를 반환하고 그렇지 않으면 좌항의 피연산자를 반환한다.

```javascript
var foo = null ?? 'default string';
console.log(foo); // "default string"
```

> 좌항의 피연산자가 false 로 평가되는 Falsy 값(false, undefined, null, 0, -0, Nan, '')을 주의하자
