# 28. Number

## 28.1 Number 생성자 함수

> Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하게 되면 [[NumberData]] 내부 슬롯에 0을 할당한 래퍼 객체를 생성한다.

```javascript
const numObj = new Number();
console.log(numObj); // Number {[[PrimitveValue]]: 0}
```

## 28.2 Number 프로퍼티

## 28.2.1 Number.EPSILON

> ES6 에서 도입된 Number.EPSILON 은 1과 1보다 큰 숫자 중에서 가장 작은 수자와의 차이와 같다.

```javascript
0.1 + 0.2;
0.1 + 0.2 === 0.3; // false
```

> 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```javascript
function isEqual(a,b) {
    return Math.abs(a - b) < Number.EPSILON
}

isEqual(0.1 + 0.2, 0.3); // true
```
### 28.2.2 Number.MAX_VALUE

> 자바스크립트에서 표현할 수 있는 가장 큰 양수 이다.

### 28.2.3 Number.MIN_VALUE

> 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.

### 28.2.4 Number.MAX_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값

### 28.2.5 Number.MIN_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값

### 28.2.6 Number.POSITIVE_INFINITY

> 무한대를 나타내는 숫자값 Infinity 와 같다.

### 28.2.7 Number.NEGATIVE_INFINITY

> 음의 무한대를 나타내는 숫자값 -Infinity 와 같다.

### 28.2.8 Number.NaN


## 28.3 Number 메서드

### 28.3.1 Number.isFinite

> 정상적인 유한수인지 체크 Infinity 인지 아닌지 체크하여 boolean 값으로 반환

### 28.3.2 Number.isInteger

> 정수인지 아닌지 체크하는 메서드

### 28.3.3 Number.isNaN

> NaN 체크 메서드

> isNaN 과 차이가 있다.

```javascript
Number.isNaN(undefined); // false

isNaN(undefined); // true
```

### 28.3.4 Number.isSafeInteger

> 인수로 전달된 숫자값이 안전한 정수인지 검사하여 boolean 값으로 반환

> 숫자로 암묵적 타입 변환이 이루어지지 않는다.

### 28.3.5 Number.prototype.toExponential

> 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

### 28.3.6 Number.prototype.toFixed

> 숫자를 반올림하여 문자열로 반환한다. 반올림하는 소수점 이하 자리수를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.

### 28.3.7 Number.prototype.toPrecision

> 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

### 28.3.8 Number.prototype.toString

> 숫자를 문자열로 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다. 인수를 생략하면 기본적으로 10진법이 지정된다.

---

생략
