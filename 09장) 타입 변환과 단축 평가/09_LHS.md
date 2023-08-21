# 9.1 타입 변환이란?
자바스크립트의 모든 값은 타입이 있다.
값의 타입은 개발자의 의도에 따라 다른 타입으로 변환할 수 있다.

개발자가 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 또는 **타입 캐스팅** 이라 한다.

```js
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅 한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x) // number 10

```

개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동변환되기도 한다.
이를 **암묵적 타입 변환** 또는 **타입 강제 변환** 이라 한다.

```js
var x = 10;

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + '';
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

중요한 것은 코드를 예측할 수 있어야 한다는 것이다.
동료가 작성한 코드를 정확히 이해할 수 있어야하고 자신이 작성한 코드도 동료가 쉽게 이해할 수 있어야한다.

# 9.2 암묵적 타입 변환
자바스크립트 엔진은 표현식을 평가할 때 개발자의 의도와는 상관없이 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환(암묵적 타입 변환)할 때가 있다.

```js
// 피연산자가 모두 문자열 타입이어야 하는 문맥
'10' + 2 // 102

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * '10' // 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0 // true
if(1) {}

```

타입별로 암묵적 타입 변환이 어떻게 발생하는지 살펴보자.

## 9.2.1 문자열 타입으로 변환
```js
// 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.
1 + '2'; // "12"

// 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.
`1 + 1 = ${1 + 1}`; // "1 + 1 = 2"
```

자바스크립트 엔진은 문자열 타입이 아닌 문자열 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작한다.

```js
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

## 9.2.2 숫자 타입으로 변환

```js
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN
```
위 예제에 사용한 연산자는 모두 산술 연산자다.
산술 연산자의 역할은 숫자 값을 만드는 것이다.
산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.
숫자타입이 아닐경우 숫자 타입으로 암묵적 타입 변환한다.

```js
'1' > 0 // true
```


자바스크립트 엔진은 숫자 타입이 아닌 값을 숫자 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 수행한다.

```js
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

'', \[\], null, false 는 0으로 true는 1로 변환된다.
객체나 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주목하자.

## 9.2.3 불리언 타입으로 변환

자바스크립트 엔진은 조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환한다.

```js
if('') console.lolg('1');
if(true) console.lolg('2');
if(0) console.lolg('3');
if('str') console.lolg('4');
if(null) console.lolg('5');

// 2 4
```

**자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 (참으로 평가되는 값)
또는 Falsy 값 (거짓으로 평가되는 값) 으로 구분한다.**

Falsy 값 : false로 평가되는 값
- false
- undefined
- null
- 0, -0
- NaN
- '' (빈문자열)

```js
// 아래 조건문은 모두 코드 블록을 실행한다.
if(!false) console.lolg('1');
if(!undefined) console.lolg('2');
if(!null) console.lolg('3');
if(!0) console.lolg('4');
if(!NaN) console.lolg('5');
if(!'') console.lolg('6');
```

Truthy 값 : Falsy값 외에 모든 값은 모두 true로 평가된다.

```js
// 전달받은 인수가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
	return !v;
}

// 전달받은 인수가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
	return !v;
}

// 모두 true를 반환한다.
isFalsy(false);
isFalsy(undefined);
isFalsy(null);
isFalsy(0);
isFalsy(NaN);
isFalsy('');

// 모두 true를 반환한다.
isTruthy(true);
isTruthy('0'); // 빈 문자열이 아닌 문자열은 Truthy 값 이다.
isTruthy({});
isTruthy([]);

```
# 9.3 명시적 타입 변환
개발자의 의도에 따라 명시적으로 타입을 변경하는 방법은 다양하다.

- 방법
    - 표준 빌트인 생성자 함수(String, Number, Boolean)를 new 연산자 없이 호출하는 방법
    - 빌트인 메서드를 사용하는 방법
    - 암묵적 타입 변환을 이용하는 방법

### 표준 빌트인 생성자 함수와 빌트인 메서드

> 표준 빌트인 생성자 함수와 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 함수다.
> 표준 빌트인 생성자 함수는 객체를 생성하기 위한 함수이며 new 연산자와 함께 호출한다.
> 표준 빌트인 메서드는 자바스크립트에서 기본 제공하는 빌트인 객체의 메서드다.

## 9.3.1 문자열 타입으로 변환
문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.
- String 생성자 함수를 new 연산자 없이 호출하는 방법
- Object.prototype.toString 메서드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법

```js
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 -> 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"
// 불리언 타입 -> 문자열 타입
String(true); // "true"
String(false); // "false"

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 -> 문자열 타입
(1).toString(); // "1"
(NaN).toString(); // "NaN"
(Infinity).toString(); // "Infinity"
// 불리언 타입 -> 문자열 타입
(true).toString(); // "true"
(false).toString(); // "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 -> 문자열 타입
1 + '';
NaN + '';
Infinity + '';
// 불리언 타입 -> 문자열 타입
true + '';
false + '';

```

## 9.3.2 숫자 타입으로 변환
숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.
1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. + 단항 산술 연산자를 이용하는 방법
4. * 산술 연산자를 이용하는 방법

```js
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 -> 숫자 타입
Number('0'); // 0
Number('-1'); // -1
Number('10.53') // 10.53
// 불리언 타입 -> 숫자 타입
Number(true) // 1
Number(false) // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 -> 숫자 타입
parseInt('0'); // 0
parseInt('-1') // -1
parseFloat('10.53') // 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 -> 숫자타입
+'0'; // 0
+'-1'; // -1
+'10.53'; // 10.53
// 불리언 타입 -> 숫자타입
+ture; // 1
+false; // 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 -> 숫자 타입
'0' * 1; // 0
'-1' * 1; // -1
'10.53' * 1; // 10.53
// 불리언 타입 -> 숫자 타입
true * 1; // 1
false * 1; // 0

```

## 9.3.3 불리언 타입으로 변환
불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.
1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
2. ! 부정 논리 연산자를 두번 사용하는 방법

```js
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// string -> boolean
Boolean('x'); // true
Boolean(''); // false
Boolean('false'); // true
// number -> boolean
Boolean(0); // false
Boolean(1); // true
Boolean(NaN); // false
Boolean(Infinity); // true
// null -> boolean
Boolean(null); // false
// undefined -> boolean
Boolean(undefined); // false
// object -> boolean
Boolean({}); // true
Boolean([]); // true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// string -> boolean
!!'x'; // true
!!''; // false
!!'false' // true
// number -> boolean
!!0; // false
!!1; // true
!!NaN; // false
!!Infinity; // true
// null -> boolean
!!null; // false
// undefined -> boolean
!!undefined; // false
// object -> boolean
!!{}; // true
!![]; // true

```

# 9.4 단축 평가
## 9.4.1 논리 연산자를 사용한 단축 평가
논리곱(&&) 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다.
논리곱 연산자는 좌항에서 우항으로 평가가 진행된다.

```js
'Cat' && 'Dog' // "Dog"
```
**논리 연산의 결과를 결정하는 두 번째 피연산자 즉 'Dog'를 그대로 반환한다.**

```js
'Cat' || 'Dog' // "Cat"
```
**논리 연산의 결과를 결정한 첫 번째 피연산자 즉 문자열 'Cat'을 그대로 반환한다.**

### 단축평가

이처럼 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다.
이를 **단축 평가**라 한다.
단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 말한다.

| 단축 평가 표현식    | 평가 결과 |
| ------------------- | --------- |
| true \|\| anything  | true      |
| false \|\| anything | anything  |
| true && anything    | anything  |
| false && anything   | false     |


### 단축평가를 이용한 유용한 패턴
**객체를 가리키기를 기대하는 변수가 Null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때**

```js
var elem = null;
var value = elem.value; // TypeError: Cannot read property 'value' of null
```
위와 같이 기대하는 변수 값이 객체가 아니라 null 또는 undefined인 경우 타입 에러가 발생한다.
이를 단축 평가를 사용하면 에러를 발생시키지 않는다.

```js
var elem = null;
// elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
// elem이 Truthy 값이면 elem.value로 평가된다.
var value = elem && elem.value; // null;
```

**함수 매개변수에 기본 값을 설정할 때**
함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다.
이때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.

```js
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

## 9.4.2 옵셔널 체이닝 연산자
ES11 에서 도입된 옵셔널 체이닝 연산자 ?. 는 좌항의 피연산자가 null 또는 undefined 인 경우 undefined를 반환하고 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```js
var elem = null;

var value = elem?.value;
console.log(value); // undfeind
```

이전에는 논리 연산자 &&를 사용하여 단축 평가를 통해 변수가 null 또는 undefined인지 확인했다.
논리 연산자는 좌항 피연산자가 Falsy 값이면 좌항 피연산자를 그대로 반환하기 때문에 다음과 같은 일이 일어날 수도 있다.

```js
var str = '';
var length = str && str.length;
console.log(length); // ''
```

하지만 옵셔널 체이닝 연산자는 피연산자가 false로 평가되는 Falsy 값 (false, undefined, null, 0, -0, NaN, '')이라도 null 또는 undefined 가 아니면 우항의 프로퍼티 참조를 이어간다.

```js
var str = '';
var length = str?.length;
console.log(length) // 0
```

## 9.4.3 null 병합 연산자
ES11에 도입된 null 병합 연산자 ?? 는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.

```js
var foo = null ?? 'default string';
console.log(foo); // "default string"
```

하지만 Falsy 값 (false, undefined, null, 0, -0, NaN, '') 이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.

```js
var foo = '' ?? 'default string';
console.log(foo); // ''
```

# 알고 가기
---
- [ ] 다음 코드의 결과값을 예측해보시오.

```js
'10' + 2 // ??

5 * '10' // ??
```
