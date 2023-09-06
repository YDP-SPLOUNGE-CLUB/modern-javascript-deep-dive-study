# 28.1 Number 생성자 함수
Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 \[\[NumberData\]\] 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.

인수로 숫자를 전달하면서 new 연산와 함께 호출하면 \[\[NumberData\]\] 내부 슬롯에 인수로 전달한 숫자를 할당한 Number 래퍼 객체를 생성한다.

인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환후, \[\[NumberData\]\] 내부 슬롯에 변화된 숫자를 할당한 Number 래퍼 객체를 생성한다.
```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
const numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
const numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

# 28.2 Number 프로퍼티
```js
// 28.2.1 Number.EPSILON (ES6)
// 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.

// 다음 예제와 같이 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵디.
0.1 + 0.2; // 0.30000..04
0.1 + 0.3 === 0.3; false

// 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환했을 때
// 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계가 있다.
function isEqual(a,b) {
	// a와 b를 뺀 값의 절대값이 Number.EPSILON 보다 작으면 같은 수로 인정한다.
	return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3); // true

// 28.2.2 Number.MAX_VALUE
// Number.MAX_VALUE 보다 큰 숫자는 Infinity다.
Infinity > Number.MAX_VALUE; // true

// 28.2.3 Number.MIN_VALUE
// JavaScript에서 표현할 수 있는 가장 작은 양수 값이다.
// Number.MIN_VALUE 보다 작은 숫자는 0 이다.
Number.MIN_VALUE > 0; // true

// 28.2.4 Number.MAX_SAFE_INTEGER
// JavaScript에서 안전하게 표현할 수 있는 가장 큰 정수값 이다.
Number.MAX_SAFE_INTEGER; // 9007199254740991

// 28.2.5 Number.MIN_SAFE_INTEGER
// JavaScript에서 안전하게 표현할 수 있는 가장 작은 정수값 이다.
Number.MIN_SAFE_INTEGER; // -9007199254740991

// 28.2.6 Number.POSITIVE_INFINITY
// 양의 무한대를 나타내는 Infinity와 같다.
Number.POSITIVE_INFINITY; // Infinity

// 28.2.7 Number.NEGATIVE_INFINITY
// 음의 무한대를 나타내는 -Infinity와 같다.
Number.NEGATIVE_INFINITY; // -Infinity

// Number.NaN
// 숫자가 아님을 나타내는 숫자값이다.
// Number.NaN 은 window.NaN과 같다.
Number.NaN; // NaN
```

# 28.3 Number 메서드
```js
// 28.3.1 Number.isFinite (ES6)
// Number.isFinite 정적 메서드는 인수로 전달된 숫자값이 정상적인 유한수
// Infinity -Infinity 가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다. 
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

// Number.isFinite와 빌트인 전역 함수 isFinite와 차이가 있다.
// Number.isFinite는 인수를 숫자로 암묵적으로 타입 변환하지 않고
// isFinite는 숫자로 암묵적 타입 변환하여 검사를 수행한다.
Number.isFinite(null); // false
// 암묵적으로 타입 변환한다.
isFinite(null); // true

// 28.3.2 Number.isinterger
// 인수로 전달된 숫자 값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
Number.isInteger(0) // true
Number.isInteger(123) // true
Number.isInteger(-123) // true

Number.isInteger(0.5) // false
// 암묵적 타입 변환하지 않는다.
Number.isInteger('123') // false
Number.isInteger(false) // false
Number.isInteger(Infinity) // false
Number.isInteger(-Infinity) // false

// Number.isNaN (ES6)
// NaN 인지 검사하여 그 결과를 불리언 값으로 반환한다.
Number.isNaN(NaN); // true

// Number.NaN 빌트인 전역 함수 NaN와 차이가 있다.
// Number.NaN 인수를 숫자로 암묵적으로 타입 변환하지 않고
// 빌트인 전역함수 NaN는 숫자로 암묵적 타입 변환하여 검사를 수행한다.
Number.isNaN(undefined); // false
// 암묵적으로 타입 변환한다.
isNaN(undefined); // true

// 28.3.4 Number.isSafeInterger (ES6)
// 인수로 전달된 숫자 값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
// 검사전에 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true
Number.isSafeInteger(10000000000000001); // false
Number.isSafeInteger(0.5); // false
Number.isSafeInteger('123'); // false
Number.isSafeInteger(false); // false
Number.isSafeInteger(Infinity); // false

// 28.3.5 Number.prototype.toExponential
// 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.
(77.1234).toExponential(); // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.71e+1"

// 28.3.6 Number.prototype.toFixed
// 숫자를 반올림하여 문자열로 반환
(12345.6789).toFixed(); // "12346"
(12345.6789).toFixed(1); // "12345.7"
// 소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(2); // "12345.68"
(12345.6789).toFixed(3); // "12345.678"

// 28.3.7 Number.prototype.toPrecision
// 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
// 인수를 생략하면 기본값 0 이 지정된다.
(12345.6789).toPrecision(); // "12345.6789"
(12345.6789).toPrecision(1); // "1e+4"
(12345.6789).toPrecision(2); // "1.2e+4"
(12345.6789).toPrecision(6); // "12345.7"

// Number.prototoype.toString
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // "10"
// 2진수 문자열을 반환한다.
(16).toString(2); // "10000"
// 8진수 문자열을 반환한다.
(16).toString(8); // "20"
// 16진수 문자열을 반환한다.
(16).toString(16); // "10"

```

# 알고 가기
---
생략
