# 7. 연산자

> 연산자는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행해 하나의 값을 만든다.

## 7.1 산술 연산자

> 산술 연산자는 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다. 산술 연산이 불가능할 경우 **NaN** 을 반환한다.

### 7.1.1 이항 산술 연산자

> 이항 산술 연산자는 2개의 피연산자를 산술 연산하여 숫자 값을 만든다.
> 피연산자의 값을 변경하는 부수 효과가 없다. 어떤 산술 연산을 해도 피연산자의 값이 바뀌는 경우는 없고 언제나 새로운 값을 만든다.

| 이항 산술 연산자 | 의미  | 부수 효과 |
|-----------|-----|-------|
| +         | 덧셈  | X     |
| -         | 뺄셈  | X     |
| *         | 곱셈  | X     |
| /         | 나눗셈 | X     |
| %         | 나머지 | X     |

### 7.1.2 단항 산술 연산자

> 단항 산술 연산자는 1개의 피연산자를 산술 연산하여 숫자 값을 만든다.

> 증가 감소 연산자는 (++ / --) 피연산자의 값을 변경하는 부수 효과가 있다.

| 단항 산술 연산자 | 의미                             | 부수효과 |
|-----------|--------------------------------|------|
| ++        | 증가                             | O    |
| --        | 감소                             | O    |
| +         | 어떠한 효과도 없다. 음수를 양수로 반전하지도 않는다. | X    |
| -         | 양수를 음수로, 음수를 양수로 반전한 값을 반환한다.  | X    |

> 증가 감소 연산자는 위치에 의미가 있다.

> 피연산자 앞에 위치한 ++ / -- 는 먼저 피연산자의 값을 증가 시킨 이후 연산을 수행한다.

```javascript
var x = 5, result;

result = ++x; // 6 6

```

> 피연산자 뒤에 위치한 ++ / -- 연산자는 피연산자의 값을 나중에 연산한다.

```javascript
var x = 5, result;

result = x++; // 5 6
```

> + 단항 연산자는 피연산자에 어떠한 효과도 없다.

### 7.1.3 문자열 연결 연산자

> + 연산자는 피연산자 중 하나 이사이 문자열인 경우 문자열 연결 연산자로 동작한다.

```javascript
'1' + 2; // 12
1 + '2'; // 12
```

> 이러한 현상을 암묵적 타입 변환 또는 타입 강제 변환 이라 한다.

## 7.2 할당 연산자

> 할당 연산자는 우항에 있는 피연산자의 평가 결과에 좌항에 있는 변수에 할당한다.

| 할당 연산자 | 예      | 동일 표현      | 부수 효과 |
|--------|--------|------------|-------|
| =      | x = 5  | x = 5      | O     |
| +=     | x += 5 | x = x + 5  | O     |
| -=     | x -= 5 | x = x - 5  | O     |
| \*=    | x\*=5  | x = x \* 5 | O     |
| /=     | x /= 5 | x = x / 5  | O     |
| %=     | x %= 5 | x = x % 5  | O     |

> 할당문은 변수에 값을 할당하는 부수 효과만 있을 뿐 값으로 평가되지 않을 것처럼 보인다. 하지만
> **할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가된다.**
> 여러 변수에 동일한 값을 연쇄 할당할 수 있다.

```javascript
var a, b, c;

a = b = c = 0;
console.log(a, b, c) // 0 0 0
```

## 7.3 비교 연산자

> 비교 연산자는 좌항 / 우항의 피연산자를 비교하고 그 결과를 boolean 값으로 반환한다.

### 7.3.1 동등 / 일치 비교 연산자

| 비교 연산자 | 의미     | 사례      | 설명              | 부수 효과 |
|--------|--------|---------|-----------------|-------|
| ==     | 동등 비교  | x == y  | x와 y의 값이 같음     | X     |
| ===    | 일치 비교  | x === y | x와 y의 값과 타입이 같음 | X     |
| !=     | 부동등 비교 | x != y  | x와 y의 값이 다름     | X     |
| !==    | 불일치 비교 | x !== y | x와 y의 값과 타입이 다름 | X     |

```javascript
5 == '5' // true
5 === '5' // false
```

> 일치 비교 연산자는 좌항과 우항의 피연산자가 타입도 같고 값도 같은 경우에 한하여 true 를 반환한다.

> 일치 비교 연산자에서 NaN 는 서로 일치하지 않는다.

```javascript
NaN === NaN // false
```

### 7.3.2 대소 관계 비교 연산자

| 대소 관계 비교 연산자 | 예제     | 설명            | 부수 효과 |
|--------------|--------|---------------|-------|
| >            | x > y  | x가 y보다 크다     | X     |
| <            | x < y  | x가 y보다 작다     | X     |
| >=           | x >= y | x가 y보다 크거나 같다 | X     |
| <=           | x <= y | x가 y보다 작거나 같다 | X     |

## 7.4 삼항 조건 연산자

> 삼항 조건 연산자는 조건식의 평가 결과에 따라 반환할 값을 결정한다. 자바스크립트의 유일한 삼항 연산자이며 부수 효과는 없다.

```javascript
var result = score > 60 ? 'pass' : 'fail';
```

> 삼항 조건 연산자 표현식은 if... else 문과 중요한 차이가 있다.
> 삼항 조건 연산자 표현식은 값처럼 사용할 수 있지만 if ... else 는 그렇지 않다.

> 조건에 따라 어떤 값을 결정해야 한다면 삼항 조건 연산자 표현식이 유리하다. 하지만 여러 조건문일 경우 if ... else 문이 가독성이 더 좋다.

## 7.5 논리 연산자

> 논리 연산자는 우항과 좌항의 피연산자를 논리 연산한다.

```javascript
true || true // true
true || false // true
false || false // false

true && true // true
true && false // false
```

| 논리 연산자 | 의미      | 부수 효과 |
|--------|---------|-------|
| \      | \       |       | 논리합 OR  | X         |
| &&     | 논리곱 AND | X     |
| !      | 부정 NOT  | X     |

> 논리 부정 연산자는 언제나 boolean 값을 반환한다. 단 피연산자가 반드시 boolean 일 필요는 없다.

```javascript
!0; // false
```

> 논리합 또는 논리곱 연산자 표현식의 평과 결과는 boolean 이 아닐 수도 있다. 두 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.

```javascript
'Cat' && 'Dog' // 'Dog'
```

## 7.6 쉼표 연산자

> 쉼표 연산자는 왼쪽 피연산자부터 차례대로 피연산자를 평가하고 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환한다.

## 7.7 그룹 연산자

> 소괄호로 피연산자를 감싸는 그룹 연산자는 자신의 피연산자인 표현식을 가장 먼저 평가한다.
> 연산자의 우선순위를 조정할 수 있다. 그룹 연산자는 연산자 우선순위가 가장 높다.

```javascript
10 * 2 + 3; // 23

10 * (2 + 3); // 50
```

## 7.8 typeof 연산자

> typeof 연산자는 피연산잦의 데이터 타입을 문자열로 반환한다.

> 'string', 'number', 'boolean', 'undefined', 'object', 'symbol', 'function'
> 중 하나를 반환한다. null 을 반환하는 경우는 없다.

## 7.9 지수 연산자

> ES7 에서 도입된 지수 연산자는 좌항의 피연산자를 밑으로 우항의 피연산자를 지수로 거듭 제곱하여 숫자 값을 반환한다.

```js
2 ** 2; // 4
2 ** 2.5; // 5.65685~~~~~
2 ** 0; // 1
2 ** -2; // 0.25
```

> 지수 연산자는 할당 연산자와 함께 사용할 수 있다.

```javascript
var num = 5;
num **= 2; // 25
```

> 지수 연산자는 이항 연산자 중에서 우선순위가 가장 높다.

```javascript
2 * 5 ** 2; // 50
```

## 7.10 그 외의 연산자

> 지금까지 소개한 연산자 외에도 다양한 연산자가 있다.

| 연산자        | 개요                                |
|------------|-----------------------------------|
| ?.         | 옵셔널 체이닝 연산자                       |
| ??         | null 병합 연산자                       |
| delete     | 프로퍼티 삭제                           |
| new        | 생성자 함수를 호출할 때 사용하여 인스턴스를 생성       |
| instanceof | 좌변의 객체가 우변의 생성자 함수와 연결된 인스턴스인지 판별 |
| in         | 프로퍼티 존재 확인                        |

## 7.11 연산자의 부수 효과

> 대부분의 연산자는 다른 코드에 영향을 주지 않는다.
> 하지만 일부 연산자는 다른 코드에 영향을 주는 부수 효과가 있다.

> 부수 효과가 있는 연산자는 할당 연산자, 증가/ 감소 연산자 , delete 연산자다.

## 7.12 연산자 우선순위

| 우선순위 | 연산자                                                       |
|------|-----------------------------------------------------------|
| 1    | ()                                                        |
| 2    | new(매개변수 존재), .. \[\](프로퍼티접근), ()(함수 호출), ?.(옵셔널 체이닝 연산자) |
| 3    | new(매개변수 미존재)                                             |
| 4    | x++, x--                                                  |
| 5    | !x, +x, -x, ++x, --x, typeof, delete                      |
| 6    | \*\*(이항 연산자 중에서 우선순위가 가장 높다)                              |
| 7    | *, /, %*                                                  |
| 8    | +, -                                                      |
| 9    | <, <=, >, >=, in, instanceof                              |
| 10   | =\=, !=\=, =\=\=, !=\=                                    |
| 11   | ??(null 병합 연산자)                                           |
| 12   | &&                                                        |
| 13   | \                                                         |\|                                                                               |
| 14   | ? ... : ...                                               |
| 15   | 할당 연산자(=, +=, -=, ...)                                    |
| 16   | ,                                                         |

## 7.13 연산자 결합 순서

연산자 결합 순서란 연산자의 어느 쪽부터 평가를 수행할 것인지를 나타내는 순서를 말한다.


| 결합 순서    | 연산자                                                                              |
|----------|----------------------------------------------------------------------------------|
| 좌항 -> 우항 | +, -, /, %, <, <=, >, >=, &&, \                                                  |\|, ., \[\], (), ??, ?., in, instanceof               |
| 우항 -> 좌항 | ++, --, 할당연산자(=, +=, ...), !x, +x, -x, ++x, --x, typeof, delete, ? ... : ..., ** |

---

생략