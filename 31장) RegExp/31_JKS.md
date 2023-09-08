# 31. RegExp

## 31.1 정규 표현식이란?

> 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
> 정규 표현식은 자바스크립트의 고유 문법이 아니며 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.
> 자바스크립트는 펄의 정규 표현식 문법을 ES3 부터 도입했다.

> 문자열을 대상으로 패턴 매칭 기능을 제공한다.

## 31.2 정규 표현식의 생성

> 정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.

> 일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다.

> 정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```javascript
const target = `Is this all there is?`;

const regxp = /is/i;

regxp.test(target); // true

const regexp2 = new RegExp(/is/i);
```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec

> exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.

```javascript
const target = `Is this all there is?`;
const regExp = /is/;

regExp.exec(target);
```

### 31.3.2 RegExp.prototype.test

> test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```javascript
const target = `Is this all there is?`;
const regExp = /is/;

regExp.test(target); // true
```

### 31.3.3 String.prototype.match

> String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```javascript
const target = `Is this all there is?`;
const regExp = /is/;

target.match(regExp);
```

## 31.4 플래그

> 패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 플래그는 6개가 있다.

| 플래그 | 의미          | 설명                                   |
|-----|-------------|--------------------------------------|
| i   | ignore case | 대소문자 구별없이 패턴을 검색한다.                  |
| g   | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m   | multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.           |

> 플래그 옵션이므로 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.
> 어떠한 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색한다.

> 그리고 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```javascript
const target = `Is this all there is?`;

target.match(/is/);

target.match(/is/i);

target.match(/is/g); 

target.match(/is/ig);
```

## 31.5 패턴

> 정규 표현식은 일정한 규칙을 가진 문자열의 잡합을 표현하기 위해 사용하는 형식 언어다.

> 패턴은 /로 열고 닫으며 문자열의 따음표는 생략한다.

### 31.5.1 문자열 검색

> 정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.

> 앞서 살펴본 RegExp 메서드를 사용하여 검색 대상 문자열과 정규 표현식의 매칭 결과를 구하면 검색이 수행된다.

```javascript
const target = 'Is this all there is?';

const regExp = /is/;

// target과 정규 표현식이 매치하는 테스트한다.
regExp.test(target);

// target 과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
```

> 대소문자를 구별하지 않고 검색하려면 플래그 i 를 사용한다.

```javascript
const target = 'Is this all there is?';

const regExp = /is/i;

target.match(regExp);
```

### 31.5.2 임의의 문자열 검색

> .은 임의의 문자 한 개의 의미한다. 문자의 내용은 무엇이든 상관없다.
> 다음 예제의 경우 .을 3개 연속해서 패턴을 생성했으므로 문자의 내용과 상관없이 3자리 문자열과 매치한다.

```javascript
const target = 'Is this all there is?';

const regExp = /.../g;
```

### 31.5.3 반복 검색

> {m, n} 은 앞선 패턴이 최소 m 번 최대 n 번 반복되는 문자열을 의미한다.

```javascript
const target = 'A AA B BB Aa Bb AAA';

const regExp = /A{1,2}/g;

target.match(regExp); // A AA A AA A
```

```javascript
const target = 'A AA B BB Aa Bb AAA';

const regExp = /A{2,}/g;

target.match(regExp);
```

> +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉 +는 {1, } 과 같다. 다음 에제의 경우 A+ 는 앞선 패턴 'A'가 한번 이상 반복되는 문자열을 매치한다.

```javascript
const target = 'A AA B BB Aa Bb AAA';

const regExp = /A+/g;

target.match(regExp); // A AA A AAA
```

> ?는 앞선 패턴이 최대 한 번 이상 반복되는 문자열을 의미한다. 즉 ?는 {0, 1} 과 같다.


```javascript
const target = 'color colour';

const regExp = /colou?r/g;

target.match(regExp); // color colour
```

### 31.5.4 OR 검색

> | 는 or 의미를 가진다.

```javascript
const target = 'A AA B BB Aa Bb';

const regExp = /A|B/g;

target.match(regExp); // A A A B B B A B  
```

> 분해되지 않은 단어 레벨로 검색하기 위해서는 + 를 함께 사용한다.

```javascript
const target = 'A AA B BB Aa Bb';

const regExp = /A+|B+/g;

target.match(regExp);
```

> \w 는 알파벳 숫자 언더스코어를 의미한다. 즉 \w [A-Za-z0-9_] 를 뜻한다.

### 31.5.5 NOT 검색

> [ ... ] 내의 ^은 not 의미를 갖는다.

```javascript
const target = 'AA 12 Aa Bb';

const regExp = /[^0-9]+/g;

target.match(regExp); // [AA, Aa Bb]
```

### 31.5.6 시작 위치로 검색

> [ ... ] 밖의 ^은 문자열의 시작을 의미한다. [ ... ] 내의 ^은 not 의미를 가지므로 주의하기 바란다.

```javascript
const target = `https://poiemaweb.com`;

const regExp = /^https/;

regExp.test(target);
```

### 31.5.7 마지막 위치 검색

> $는 문자열의 마지막을 의미한다.

```javascript
const target = `https://poiemaweb.com`;

const regExp = /com$/;

regExp.test(target); // true
```

## 31.6 자주 사용하는 정규표현식

### 31.6.1 특정 단어로 시작하는지 검사

```javascript
const url = 'https://example.com';

/^https?:\/\//.test(url);
```

### 31.6.2 특정 단어로 끝나는지 검사

> 다음 예제는 검색 대상 문자열이 'html'로 끝나는지 검사한다.

```javascript
/html$/.test(fileName);
```

### 31.6.3 숫자로만 이루어진 문자열인지 검사

```javascript
const target = '12345';

/^\d+$/.test(target);
```

### 31.6.4 하나 이상의 공백으로 시작하는지 검사

> \s 는 여러 가지 공백 문자를 의미한다.

```javascript
const target = `Hi!`;

/^[\s]+/.test(target);
```

### 31.6.5 아이디로 사용 가능한지 검사

```javascript
const id = 'abc123';

/^[A-Za-z0-9]{4,10}$/.test(id); // true
```

### 31.6.6 메일 주소 형식에 맞는지 검사

```javascript
const email = 'ungmo2@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email);
```

---

- [ ] 특수 문자 포험 여사 검사 정규식 작성
- [ ] 메일 주소 형식에 맞는지 검사