# 32. String

## 32.1 String 생성자 함수

> 표준 빌트인 객체인 String 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출되어 String 인스턴스를 생성할 수 있다.

> String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출함ㄴ [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.

> String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 수자 형식의 무자열을 프로퍼티 키로
> 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.

> String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후
> [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```javascript
let strObj = new String(123);
console.log(strObj);
// String {0: "1", 1: "2", 2 : "3", length: 3, [[PrimitiveValue]]: "123"}

strObj = new String(null);
console.log(strObj);
// String {0: "n", 1: "u", 2 : "l", 3: "l", length: 4, [[PrimitiveValue]]: "null"}
```

## 32.2 length 프로퍼티

## 32.3 String 메서드

> 배열에는 원본 배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있다.

> String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다.

> 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.

### 32.3.1 String.prototype.indexOf

> indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 실패하면 -1를 반환한다.

```javascript
const str = "Hello world";

str.indexOf('l'); // 2
```

### 32.3.2 String.prototype.search

> search 메서드는 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 문자열의 인덱스를 반환한다. 실패하면 -1를 반환한다.

```javascript
const str = 'Hello world';

str.search(/o/); // 4
```

### 32.3.3 String.prototype.includes

> 대상 문자열에 인수로 전다받은 문자열이 포함되어 있는지 확인하여 결과를 boolean 값으로 반환한다.

### 32.3.4 String.prototype.startsWith

> 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 boolean 값으로 반환한다.
> startsWith 메서드의 2번째 인수로 검색을 시작한 인덱스를 전달할 수 있다.

### 32.3.5 String.prototype.endsWith

> 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인 후 boolean 값으로 반환한다.

### 32.3.6 String.prototype.charAt

> 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색, 반환한다.

### 32.3.7 String.prototype.substring

> substring 메서드는 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은
> 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.

```javascript
const str = 'Hello world';

str.substring(1, 4); // ell
```

> subString 메서드의 첫 번째 인수는 두 번째 인수보다 작은 정수이어야 정상이다.
> 하지만 다음과 같이 전달하여도 정상 동작한다.

1. 첫 번째 인수 > 두 번째 인수인 경우 두 민수는 교환된다.
2. 인수 < 0 또는 NaN 인 경우 0 으로 취급
3. 인수 > 문자열 길이 인 경우 인수는 문자열의 길이로 취급된다.

### 32.3.8 String.prototype.slice

> slice 메서드는 substring 메서드와 동일하게 동작한다. 단 slice 메서드에는 음수인 인수를 전달할 수 있다.
> 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```javascript
const str = 'hello world';

str.substring(0 ,5); // hello
str.slice(0 ,5); // hello

str.slice(-5); // world
```

### 32.3.9 String.prototype.toUpperCase

> 모든 문자열을 모두 대문자로 변겨안 문자열로 반환한다.

### 32.3.10 String.prototype.toLowerCase

> 모든 문자열을 모두 소문자로 변경한 문자열로 반환한다.

### 32.3.11 String.prototype.trim

> trim 메서드는 대상 모든 문자열 앞뒤에 공백 문자를 제거한 문자열을 반환한다.

> String.prototype.trimStart, trimEnd 를 사용하면
> 대상 문자열 앞 또는 뒤 공백 문자가 있을 경우 제거한 문자열을 반환한다.

### 32.3.12 String.prototype.repeat

> 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.

### 32.3.13 String.prototype.replace

> 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

```javascript
const str = 'Hello Hello';

str.replace(/hello/gi, 'Lee');
```

### 32.3.14 String.prototype.split

> split 메서드는 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후
> 분리된 각 문자열로 이루어진 배열을 반환한다.

```javascript
const str = 'How are you doing';

str.split(' '); // [how, are, you, doing]

str.split(/\s/); // [how, are, you, doing]

str.split(''); // [h,o,w,a,r,e,y,o,u,d,o,i,n,g]

str.split(' ', 3); // 배열의 길이를 지정 가능하다
```

---

생략


