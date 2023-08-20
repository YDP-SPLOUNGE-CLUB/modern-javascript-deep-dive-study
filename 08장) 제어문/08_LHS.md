
제어문(control flow statement)은 조건에 따라 코드 블록을 실행하거나 반복실행할 때 사용한다.

# 8.1 블록문
블록문(block statement / compound statement)은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.

```js
// 블록문
{
	var foo = 10;
}

// 제어문
var x = 1;
if( x < 10 ) {
	x++;
}

// 함수 선언문
function sum(a, b) {
	return a + b;
}
```

# 8.2 조건문
조건문은 주어진 조건식의 평가 결과에 따라 코드블록의 실행을 결정한다.

## 8.2.1 if...else 문
if 문의 조건식이 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정한다.

```js
if(조건식1) {
	// 조건식1이 참일 때 이 코드 블록이 실행된다.
} else if (조건식2) {
	// 조건식2이 참일 때 이 코드 블록이 실행된다.
} else {
	// 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}
```

## 8.2.2 switch 문
switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.
case 문은 상황을 의미하는 표현식을 지정하고 콜론으로 마친다.
switch 문의 표현식과 일치하는 case문이 없다면 실행 순서는 default문으로 이동한다.
default 문은 선택사항으로 사용할 수도 있고 사용하지 않을 수도 있다.

```js
switch(표현식) {
	case 표현식1:
		문의 표현식과 표현식1이 일치하면 실행될 문;
		break;
	case 표현식2:
		문의 표현식과 표현식2이 일치하면 실행될 문;
		break;
	default:
		문의 표현식과 일치하는 case문이 없을 때 실행될 문;
}
```

**폴스루(fall through)**
#폴스루 #FallThrough
>switch 문을 탈출하지 않고 switch문이 끝날 때까지 이후의 모든 case문과 default문을 실행하는 것

default 문에는 break 문을 생략하는 것이 일반적이다.

break 문을 생략한 폴스루가 유용한 경우도 있다.
```js
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch(month) {
	case 1: case 3: case 5: case 7: case 8: case 10: case 12:
		days = 31;
		break;
	case 4: case 6: case 9: case 11:
		days = 30;
		break;
	case 2:
		days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
		break;
	default:
		console.log('Invalid month');
}
```

만약 if else 문으로 해결할 수 있다면 switch 문보다 if else 문을 사용하는 편이 좋다.

# 8.3 반복문
조건식의 평가 결과가 참인 경우 코드 블록을 실행한다.

## 8.3.1 for 문

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
	// 조건식이 참인 경우 반복 실행될 문;
}
```

```js
// for 문의 실행 순서
for(var i = 0; i < 2; i++) {
	console.log(i);
}
```

| var i = 0 | i < 2 | i ++ | console.log(i) |
| --------- | ----- | ---- | -------------- |
| 1         |       |      |                |
|           | 2     |      |                |
|           |       |      | 3              |
|           |       | 4    |                |
|           | 5     |      |                |
|           |       |      | 6              |
|           |       | 7    |                |
|           | 8     |      |                | 

## 8.3.2 while 문
while 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
반복 횟수가 불명확할 때 주로 사용한다.

무한루프를 탈출하기 위해서는 코드 블록 내에 break문으로 탈출한다.

```js
var count = 0;

while(true) {
	count++;
	if(count === 3) break;
}
```

## 8.3.3 do...while 문
코드 블록을 먼저 실행하고 조건식을 평가한다.
무조건 한번 이상 실행된다.

```js
var count = 0;
do {
	count++;
} while(count < 3);
```

## 8.4 break문
switch 문과 while 문에서 살펴보았듯이 break문은 코드 블록을 탈출한다.
정확히는 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문(for, for...in, for...of, while, do...while) 또는 switch문의 코드 블록을 탈출한다.

레이블문(label statement)

```js
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log('foo');

// foo라는 레이블 식별자가 붙은 레이블 블록문
foo: {
	console.log(1);
	break foo; // foo 레이블 블록문을 탈출한다.
	console.log(2);
}

console.log('Done!');
```

레이블 문은 중첩된  for문 외부로 탈출할 때 유용하지만 그밖의 경우에는 일반적으로 권장하지 않는다.
레이블 문을 사용하면 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류를 발생시킬 가능성이 높아진다.

# 8.5 continue 문
반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행흐름을 이동시킨다.
```js
var string = 'Hello Wrold';
var search = 'l';
var count = 0;

for (var i = 0; i < string.length; i++) {
	if(string[i] !== search) continue;
	count++;
}
console.log(count) // 3;
```

```js
// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g');
console.log(string.atch(regexp).length); // 3
```

# 알고 가기
---
생략
