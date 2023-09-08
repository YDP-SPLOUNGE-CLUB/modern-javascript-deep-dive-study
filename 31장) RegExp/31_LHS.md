# 31.1 정규 표현식이란?

ES3 부터 도입되었으며 문자열을 대상으로 패턴 매칭 기능을 제공한다.

```js
const tel = '010-1234-567팔'
const regExp = /^\d{3}0\d{4}-\d{4}$/;
regExp.test(tel); // false
```

# 31.2 정규 표현식의 생성
정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```text
/ : 시작기호
regexp : 패턴(pattern)
/ : 종료기호
i : 플래그 (flag)

/regex/i
```

```js
const target = 'Is this all there is?';

// pattern : is
// flag : i => 대소문자를 구별하지 않고 검색
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를
// 불리언 값으로 반환한다.
regexp.test(target); // true
```

RegExp 생성자 함수 사용하여 RegExp 객체를 생성할 수도 있다.

```text
pattern: 정규 표현식 패턴
flags: 정규 표현식의 플래그(g, i, m, u, y)

new RegExp(pattern[, falgs])
```

```js
const target = 'Is this all there is?';
const regexp = new RegExp(/is/i); // ES6

regexp.test(target); // true
```

RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.

```js
const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;

count('Is this all there is?', 'is'); // 3
count('Is this all there is?', 'xx'); // 0
```

# 31.3 RegExp 메서드
## 31.3.1 RegExp.prototype.exec

인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
매칭 결과가 없는 경우 null을 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

exec 메서드는 모든 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의하기를 바란다.

## 31.3.2 RegExp.prototype.test
인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // true
```

## 31.3.3 String.prototype.match

String 표준 빌트인 객체가 제공하는 match 메서드는
대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환한다.
하지만 String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/g;

target.match(regExp); // ["is", 'is']
```


# 31.4 플래그
#플래그

| 플래그 | 의미        | 설명                                                            |
| ------ | ----------- | --------------------------------------------------------------- |
| i      | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.                       |
| g      | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검사한다. |
| m      | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.                  |

```js
const target = 'Is this all there is?';

taget.match(/is/);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]

taget.match(/is/i);
// ['is', index: 0, input: 'Is this all there is?', groups: undefined]

taget.match(/is/g);
// ['is', 'is']

taget.match(/is/ig);
// ['Is', 'is', 'is']
```

# 31.5 패턴
정규표현식은 일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
패턴을 표기하는 몇 가지 방법을 살펴보자.

## 31.5.1 문자열 검색
```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(taget); // true
taget.match(regExp);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

```js
const target = 'Is this all there is?';
const regExp = /is/i;

taget.match(regExp);
// ['is', index: 0, input: 'Is this all there is?', groups: undefined]
```

```js
const target = 'Is this all there is?';
const regExp = /is/ig;

target.match(regExp); // ['is', 'is', 'is']
```

## 31.5.2 임의의 문자열 검색
.은 임의의 문자 한 개를 의미한다.
내용은 무엇이든 상관없다.
...을 3개 연속하여 패턴을 생성하면 3자리 문자열과 매치한다.

```js
const target = 'Is this all there is?';
const regExp = /.../g;

target.match(regExp);
// ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

## 31.5.3 반복 검색
{m,n} 은 앞선 패턴이 최소 m번 최대 n번 반복되는 문자열을 의미한다.
콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의해야한다.

```js
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{1,2}/g;

// A가 2번 반복되는 문자열을 전역 검색한다.
target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

```js
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{2}/g;

// A가 최소 2번 반복되는 문자열을 전역 검색한다.
target.match(regExp); // ['AA', 'AA']
```

```js
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{2,}/g;

// A가 최소 2번 반복되는 문자열을 전역 검색한다.
target.match(regExp); // ['AA', 'AAA']
```

\+ 는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
즉 \+ 는 {1, }과 같다.

```js
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A+/g;

// A가 최소 한 번 이상 반복되는 문자열을 검색한다.
target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

? 는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다.
즉 ? 는 {0, 1} 과 같다.

```js
const target = 'color';
const regExp = /colou?r?/g;
// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고
// 'r'이 이어지는 문자열 'color', 'colour'와 매치한다.

target.match(regExp); // ["color", "colour"]
```

## 31.5.4 OR 검색

\| 는 or의 의미를 갖는다.

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

분해되지 않는 단어 레벨로 검색하기 위해서는 +를 함께 사용한다.

```js
const target = 'A AA B BB Aa Bb';

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+|/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

위 예제를 간단히 표현하면 다음과 같다.
\[ \] 내의 모든 문자는 or로 동작한다. 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.

```js
const target = 'A AA B BB Aa Bb';

const regExp = /[AB]+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```

범위를 지정하려면 \[ \] 내에 - 를 사용한다.

```js
const target = 'A AA B BB Aa Bb';
const regExp = /[A-Z]+/g;

target.match(regExp); // ["A", "AA", "BB", "ZZ", "A", "B"]
```

대소문자 구별하지 않고 알파벳을 검색하는 방법

```js
const target = 'AA BB Aa Bb 12';
const regExp = /[A-Za-z]+/g;

target.match(regExp); // ["AA", "BB", "Aa", "Bb"]
```

숫자를 검색하는 방법

```js
const target = 'AA BB 12,345';

const regExp = /[0-9]+/g;
target.match(regExp); // ["12", "345"]
```

위 예제를 간단히 표현하면 다음과 같다.
\\d 는 숫자를 의미한다.
\\d 는 \[0-9\]와 같다.
\\D 는 문자를 의미한다.
\\D 는 숫자가 아닌 모든 문자를 의미한다.

```js
const target = 'AA BB 12,345';
// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // ["12,345"]
// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;
target.match(regExp); // ["AA BB ", ","]
```

\\w 는 알파벳, 숫자, 언더스코어를 의미한다. \[A-Za-z0-9_]와 같다.
\\W 는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.

```js
const target = 'Aa Bb 12,345 _$%%';

let regExp = /[\w,]+/g;
target.match(regExp); // ["Aa", "Bb", "12,345", "_"]

regExp = /[\W,]+/g;

target.match(regExp); // [" ", " ", ",", " $%&"]
```

## 31.5.5 NOT 검색
\[...\] 내의 ^은 not의 의미를 갖는다.
예 : \[^0-9] 는 숫자를 제외한 문자, \\D 와 같다.

```js
const target = 'AA BB 12 Aa Bb';
const regExp = /[^0-9]+/g;
target.match(regExp); // ["AA BB ", " Aa Bb"]
```

## 31.5.6 시작 위치로 검색
\[...] 밖의 ^은 문자열의 시작을 의미한다.

```js
const target = 'https://naver.com';
const regExp = /^https/;
regExp.test(target); // true
```

## 31.5.7 마지막 위치로 검색
$는 문자열의 마지막을 의미한다.

```js
const target = 'https://naver.com';
const regExp = /com$/;
regExp.test(taget); // true
```

# 31.6 자주 사용하는 정규 표현식
## 31.6.1 특정 단어로 시작하는지 검사
```js
/^https?:\/\//.test('https://naver.com'); // true
/^(http|https):\/\//.test('https://naver.com'); // true
```
## 31.6.2 특정 단어로 끝나는지 검사
```js
/html$/.test('index.html'); // true
```
## 31.6.3 숫자로만 이루어진 문자열인지 검사
```js
/^\d+$/.test('1234'); // true
```
## 31.6.4 하나 이상의 공백으로 시작하는지 검사
```js
/^[\s]+/.test(' Hi');
```
## 31.6.5 아이디로 사용 가능한지 검사
```js
// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4-10자리인지 검사한다.
/^[A-ZA-Z0-9]{4, 10}$/.test('abc123');
```
## 31.6.6 메일 주소 형식에 맞는지 검사
```js
/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\._]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // true

// 인터넷 메시지 형식 규약인 RFC 5322에 맞는 정교한 패턴 매칭이 필요하다면 복잡한 패턴이 필요하다.
```
## 31.6.7 핸드폰 번호 형식에 맞는지 검사
```js
/^\d{3}-\d{3,4}-\d{4}$/.test('010-1234-5678');
```
## 31.6.8 특수 문자 포함 여부 검사
```js
const target = 'abc#123';
// 특수문자는 A-Za-z0-9 이외의 문자다.
(/[^A-Za-z0-9]/gi).test(target); // true
```

이 방식은 특수 문자를 선택적으로 검사할 수 있다는 장점이 있다.
```js
(/\[\{\}\[\]/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"`/gi).tset(taget); // true
```

특수문자를 제거할 때는 String.prototype.replace 메서드를 사용한다.
```js
target.replace(/[^A-Za-z0-9]/gi, '')
```

# 알고 가기
---
- [ ] [Regex 퀴즈 풀어보기](https://regexone.com/)
