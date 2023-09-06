# 27.1 배열이란?
배열은 여러 개의 값을 순차적으로 나열한 자료구조다.
배열이 가지고 있는 값을 **요소(element)** 라고 부른다.
배열의 요소는 배열에서 자신의 위치를 나태는 0 이상의 정수인 **인덱스(index)** 를 갖는다.
배열의 길이를 나타내는 **length 프로퍼티**를 갖는다.

```js
// 요소에 접근할 때는 대괄호 표기법을 사용한다.
arr[0] // 'apple'
arr[3] // 'orange'
// 요소 개수는 length 프로퍼티 사용한다.
arr.length // 3
```

자바스크립트에 배열이라는 타입은 존재하지 않는다.
**배열은 객체 타입이다.**

배열은 `배열 리터럴`, `Array 생성자 함수`, `Array.of`, `Array.from` 메서드로 생성할 수 있다.
배열의 프로토타입 객체는 Array.prototype 이다. Array.prototype은 배열을 위한 빌트인 메서드를 제공한다.

배열은 객체지만 일반 객체와는 구별되는 독특한 특징이 있다.

| 구분            | 객체                      | 배열          |
| --------------- | ------------------------- | ------------- |
| 구조            | 프로퍼티 키와 프로퍼티 값 | 인덱스와 요소 |
| 값의 참조       | 프로퍼티 키               | 인덱스        |
| 값의 순서       | X                         | O             |
| length 프로퍼티 | X                         | O             |

명확한 차이는 **값의 순서**와 **length 프로퍼티**다.

# 27.2 자바스크립트 배열은 배열이 아니다.
배열의 요소는 하나의 데이터 타입으로 통일되어 있으며 서로 연속적으로 인접해있다.
이러한 배열을 **밀접배열(dense array)** 이라한다.

각 요소가 동일한 데이터 크기를 가지며, 빈틈없이 연속적으로 이어져있으므로 다음과 같이 인덱스를 통해
단 한번의 연선으로 임의의 요소에 접근(임의 접근, 시간복잡도O(1))할 수 있다.
이는 매우 효율적이며, 고속으로 동작한다.
> 검색 대상 요소의 메모리 주소 = 배열의 시작 메모리 주소 + 인덱스 * 요소의 바이트 수

정렬되지 않은 배열은 특정 요소를 발견할때까지 차례대로 검색(선형검색, 시간복잡도 O(n))해야 한다.

```js
function linearSearch(arr, target) {
	const length = arr.length;
	for(let i = 0; i < length; i++) {
		if(arr[i] == target) return i;
	}
	return -1;
}
console.log(linearSearch[1,2,3,4,5,6], 3); //2
console.log(linearSearch[1,2,3,4,5,6], 0); // -1
```

또한 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 하는 단점도 있다.

또한 연속적으로 이어져 있지 않은 배열을 희소배열이라고 한다.

이처럼 자바스크립트의 배열은 엄밀히 말해 일반적인 의미의 배열이 아니다.
**자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.**
```js
console.log(Object.getOwnPropertyDescriptor([1,2,3]));
/*
	'0': {value: 1, writable: true, enumerable: true, configurable: true}
	'1': {value: 2, writable: true, enumerable: true, configurable: true}
	'2': {value: 3, writable: true, enumerable: true, configurable: true}
	length: {value: 3, writable: true, enumerable: false, configurable: false}
*/
```

**이처럼 자바스크립트 배열은 인덱스를 나타내는 문자열 프로퍼티 키로 가지며
length 프로퍼티를 갖는 특수한 객체다.
자바스크립트 배열의 요소는 사실 프로퍼티 값이다.**

자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있다.

## 일반적인 배열과 자바스크립트 배열의 장단점
- 일반적인 배열은 인덱스 요소로 빠르게 접근할 수 있다. 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.
- **자바스크립트 배열은 해시 테이블로 구현된 객체**이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없는 구조적인 단점이 있다. 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

**자바스크립트 배열은 인덱스로 접근하는 경우의 성능 대신
특정 요소를 탐색하거나 배열 요소를 삽입/삭제 하는 경우의 성능을 선택한 것이다.**

인덱스로 배열 요소에 접근할 때 일반적인 배열보다 느릴 수 밖에 없는 구조적인 단점을 보완하기 위해
대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구별하여 좀 더 배열처럼 동작하도록 최적화하여 구현하였다.
배열과 일반 객체의 성능을 테스트해보면 **일반 객체보다 약 2배정도 빠르다.**

# 27.3 Length 프로퍼티와 희소 배열

length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있다.

```js
const arr = [1, 2, 3, 4, 5];
console.log(arr.length); // 5
// 5보다 작은 숫자를 할당
arr.length = 3;
// 배열의 길이가 5에서 3으로 줄어든다.
console.log(arr); // [1, 2, 3]
```

```js
const arr = [1];
console.log(arr.length); // 1
// 1보다 큰 숫자를 할당
arr.length = 3;
// length 프로퍼티 값은 변경되지만 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty * 2]
```

위의 empty * 2 는 실제 추가된 배열의 요소가 아니다.
따라서 값이 존재하지 않는다.

이처럼 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 한다.
**자바스크립트는 희소 배열을 문법적으로 허용한다.**
```js
const sparse = [, 2, , 4];
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 요소가 존재하지 않는다.
console.log(Object.getPropertyDescriptors(sparse));
/*
	'1': ...
	'3': ...
	'length': ...
*/
```

일반적인 배열의 length는 배열의 길이와 언제나 일치한다.
**희소 배열은 length 와 배열 요소의 개수가 일치하지 않는다.
희소 배열의 length 는 희소배열의 실제 요소 개수보다 언제나 크다.**

최적화가 잘 되어 있는 모던 자바스크립트 엔진은 요소의 타입이 일치하는 배열을 생성할 때
일반적인 의미의 배열처럼 연속된 메모리 공간을 확보하는 것으로 알려져있다.

**배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다.**

# 27.4 배열 생성
```js
// 27.4.1 배열 리터럴
const arr = [];
const arr = [1,2,3];
const arr = [1, ,3]; // 희소 배열

// 27.4.2 Array 생성자 함수
const arr = new Array(10);
console.log(arr); // [empty * 10] (희소 배열)
// 배열은 최대 0 ~ 4,294,967,295 개 가질 수 있다. (배열최대개수)
// 0 ~ 2^32 -1
new Array(4294967295);
new Array(); // []
new Array(1,2,3); // [1, 2, 3]
new Array({}); // [{}]
// new 생략도 가능하다. new.target을 확인하기 때문
Array(1, 2, 3); // [1, 2, 3]

// 27.4.3 Array.of (ES6)
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of('string'); // ['string']

// 27.4.4 Array.from (ES6)
// 유사배열 객체 또는 이터러블 객체를 인수로 전달받는다.
// 유사배열 객체 반환
Array.from({length:2, 0: '1', 1: 'b'}); // ['a', 'b']
// 이터러블
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']


```

### 유사 배열 객체와 이터러블 객체
#유사배열객체 #이터러블객체 #이터러블 #유사배열

유사 배열 객체(array-like object)는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고
length 프로퍼티를 갖는 객체를 말한다.
유사 배열 객체는 마치 배열처럼 for 문으로 순회할 수도 있다.
```js
// 유사 배열 객체
const arrayLike = {
	'0': 'apple',
	'2': 'banana',
	'1': 'orange',
	length: 3
};
```

이터러블 객체(iterable object)는 Symbol.iterator 메서드를 구현하여 for...of 문으로 순회할 수 있으며
스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있는 객체를 말한다.
ES6에서 제공하는 빌트인 이터러블은 Array, String, Map, Set, Dom 컬렉션(NodeList, HTMLCollection), arguments 등이 있다.

# 27.5 배열 요소의 참조
배열의 요소를 참조할 때에는 대괄호 표기법을 사용한다.
정수로 평가되는 표현식이라면 인덱스 대신 사용할 수 있다.

```js
const arr = [1, 2];
console.log(arr[0]); // 1
console.log(arr[2]); // undefined
const arr = [1, , 3];
console.log(arr[1]); // undefined
console.log(arr[3]); // undefined
```

# 27.6 배열 요소의 추가와 갱신
객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있다.

```js
const arr = [0];
arr[1] = 1;
console.log(arr); // [0, 1]
console.log(arr.length); // 2

arr[100] = 100;
console.log(arr); // [0 ,1, empty * 98, 100]
console.log(arr.length); // 101

// 갱신
arr[1] = 10;
console.log(arr); // [0 ,10, empty * 98, 100]
```

인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수(또는 정수 형태의 문자열)를 사용해야 한다.
만약 정수 이외의 값을 인덱스 처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다.
이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.

```js
const arr = [];
// 배열 요소의 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo:3, bar: 4, '1.1': 5, '-1':6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

# 27.7 배열 요소의 삭제
배열은 사실 객체이기 때문에 배열의 특정 요소를 삭제하기위해 delete 연산자를 사용할 수 있다.
```js
const arr = [1, 2, 3];

delete arr[1];
console.log(arr); // [1, empty, 3];
// length 영향을 주지 않는다. 희소 배열이 된다.
console.log(arr.length); // 3
```

따라서 delete 연산자를 사용하지 않는것이 좋다.

```js
const arr = [1, 2, 3];
arr.splice(1, 1);
console.log(arr); // [1, 3]
console.log(arr.length); // 2
```

# 27.8 배열 메서드
Array 생성자 함수는 정적 메서드를 제공하며
배열 객체의 프로토타입인 Array.prototype 은 프로토타입 메서드를 제공한다.

배열에는 **원본 배열**(배열 메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을
**직접 변경하는 메서드** 와 **원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환**하는 메서드가 있다.

직접 변경하는 메서드
```js
// push 는 원본 배열을 직접 변경한다.
const arr = [1];
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2, 3]
```

사용 빈도가 높은 메서드에 대해 살펴보자
```js
// 27.8.1 Array.isArray
// 전달된 인수가 배열이면 true 아니면 false
// true
Array.isArray([]);
Array.isArray([1,2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ 0: 1, length: 1});


// 27.8.2 Array.prototype.indexOf
// 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환 없다면 -1 반환
const arr = [1, 2, 2, 3];
arr.indexOf(2); // 1;
arr.indexOf(4); // -1
arr.indexOf(2, 2); // 2

// 특정 요소가 존재하는지 확인할 때 유용하다.
const foods = ['a', 'b', 'o'];
if(foods.indexOf('o') === -1) {
	foods.push('o')
}
console.log(foods); // ['a', 'b', 'o']

// ES6 includes
const foods = ['a', 'b', 'o'];
if(foods.includes('o')) {
	foods.push('o')
}
console.log(foods); // ['a', 'b', 'o']

// 27.8.3 Array.prototype.push
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열 arr 마지막 요소로 추가하고 변경된 length 갑을 반환한다.
let result = arr.push(3, 4);
console.log(result); // 4

// push 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1,2,3,4]

// push 메서드는 성능 면에서 좋지 않다
// 추가할 요소가 하나라면 length 프로퍼티를 사용하여 마지막 요소를 직접 추가하는것이 더 빠르다.
const arr = [1, 2];
arr[arr.length] = 3;
console.log(arr); // [1, 2, 3]

// push 는 부수효과가 있어 스프레드 문법을 사용하는 편이 좋다.
const arr = [1, 2];
const newArr = [...arr, 3];
console.log(arr); // [1, 2, 3]

// 27.8.4 Array.prototype.pop
const arr = [1, 2];
// 마지박 요소를 반환한다.
let result = arr.pop();
console.log(result); // 2

// pop은 원본 배열을 직접 변경한다.
console.log(arr); // [1]

// 27.8.5 Array.prototype.unshift

const arr = [1, 2];
// 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다.
let result = arr.unshift(3, 4)
console.log(result); // 4
// 원본 배열을 직접 변경한다.
console.log(arr); // arr [3, 4, 1, 2]

const arr = [1, 2];
const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]

// 27.8.6 Array.prototype.shift
const arr = [1, 2];
// 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
let result = arr.shift();
console.log(result); // 1
// 원본 배열을 직접 변경한다.
console.log(arr); // [2]

// 27.8.7 Array.prototype.concat
const arr1 = [1, 2];
const arr2 = [3, 4];
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

result = arr1.concat(3);
console.log(result) // [1, 2, 3]

result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]

// push와 unshift는 concat 메서드로 대체할 수 있다.
// unshift 대체
let result = [1, 2].concat(arr2);
// push 대체
let result = arr1.concat(5, 6);

// concat은 전달받은 배열을 해체하여 새로운 배열의 요소로 추가한다.
const arr = [3, 4];
arr.unshift([1, 2]);
arr.push([5, 6]);
console.log(arr); // [[1, 2], 3, 4, [5, 6]]

let result = [1, 2].concat([3, 4]);
result = result.concat([5, 6]);
console.log(result); // [1, 2, 3, 4, 5, 6]

// concat은 ES6의 스프레드 문법을 대체할 수 있다.
result = [...[1,2], ...[3,4]];
// push/unshift/concat 메서드보다 ES6 문법을 일관성있게 사용할 것을 권장한다

// 27.8.8 Array.prototype.splice
// 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다.
const arr = [1, 2, 3, 4];
// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);
console.log(result); // [2, 3]
// 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]

const arr = [1, 2, 3, 4];
const result = arr.splice(1, 0, 100);
console.log(arr); // [1, 100, 2, 3, 4]
console.log(result); // []

const arr = [1, 2, 3, 4];
// 1부터 2개의 요소 제거
const result = arr.splice(1, 2);

console.log(arr); // [1, 4]
console.log(result); // [2, 3]

const arr = [1, 2, 3, 4];
const result = arr.splice(1);
// 원본 배열이 변경된다.
console.log(arr); // [1]
// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3, 4]

// 배열의 특정요소 제거
const arr = [1,2,3,1,2];
function remove(arr, item) {
	const index = arr.indexOf(item);
	if(index !== -1) array.splice(index, 1);
	return arr;
}
console.log(remove(arr, 2)) // [1,3,1,2]

// filter는 중복된 경우 모두 제거된다.
const arr = [1,2,3,1,2];
function removeAll(arr, item) {
	return arr.filter(v => v !== item);
}
console.log(RemoveAll(arr, 2)); // [1,3,1]


// 27.8.9 Array.prototype.slice
// slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.
// 원본 배열은 변경되지 않는다.
const arr = [1,2,3];
arr.slice(0, 1); // [1]
// arr[1]부터 arr[2] 이전 (arr[2]미포함)까지 복사하여 반환한다.
arr.slice(1, 2); // [2]
// 원본은 변경되지 않는다.
console.log(arr);

// arr[1]부터 이후의 모든 요소를 복사하여 반환한다.
arr.slice(1); // [2,3]

const arr = [1, 2, 3];
// 배열의 끝에서 요소를 한개 복사하여 반환한다.
arr.slice(-1); // [3]
arr.slice(-2); // [2, 3]
// 모두 생략하면 배열을 복사하여 복사본을 생성한다.
const copy = arr.slice();
console.log(copy === arr); // false
// 복사본은 얕은 복사를 통해 생성된다.
const todos = [
	{ id: 1, content: 'HTML', complted: false },
	{ id: 2, content: 'CSS', complted: true },
	{ id: 3, content: 'Javascript', complted: false }
];

const _todos = todos.slice();
// _todos = [...todos];
console.log(_todos === todos); // false
console.log(_todos[0] === todos[0]) // true

// 유사배열객체를 배열로 변환
function sum() {
	var arr = Array.prototypr.call(arguments);
	return arr.reduce(/*...*/)
}

function sum() {
	var arr = Array.from(arguments);
	return arr.reduce(/*...*/)
}

// arguments 객체는 유사 배열 객체이면서 이처러블 객체이다.
// 이터러블 객체는 스프레드 문법을 사용하여 배열로 변환할 수 있다.
function sum() {
	var arr = [...arguments];
	return arr.reduce(/*...*/)
}

// 27.8.10 Array.prototype.join
const arr = [1, 2, 3, 4];
arr.join(); // 1,2,3,4
arr.join(''); // 1234
arr.join(':'); // 1:2:3:4

// 27.8.11 Array.prototype.reverse
const arr = [1, 2, 3];
const result = arr.reverse();
// reverse 메서드는 원본 배열을 직접 변경한다.
console.log(arr);
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]

// 27.8.12 Array.prototype.fill (ES6)
// fill 메서드는 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
// 이때 원본 배열이 변경된다.
const arr = [1, 2, 3];
arr.fill(0);
console.log(arr); // [0,0,0]

const arr = [1, 2, 3];
// 0인수로 전달받은 값 0 을 배열의 인덱스 1부터 끝가지 요소로 채운다.
arr.fill(0, 1);
console.log(arr); // [1, 0, 0]

const arr = [1, 2, 3, 4, 5];
// 인수로 전달받은 값 0을 배열의 인덱스 1부터 3이전까지 요소로 채운다.
arr.fill(0, 1, 3);
console.log(arr); // [1, 0, 0, 4, 5]

const arr = new Array(3);
console.log(arr); // [empty * 3]
const result = arr.fill(1);
console.log(arr); // [1,1,1]
console.log(result); // [1,1,1]

const sequences = (length = 0) => Array.form({length}, (_, i) => i);
console.log(sequences(3)); // [0, 1, 2]

// 27.8.13 Array.prototype.includes
const arr = [1, 2, 3];
arr.includes(2); // true
arr.includes(100); // false
// 배열의 요소 1이 포함되어 있는지 인덱스 1부터 확인한다.
arr.includes(1, 1); // false
// 배열 요소 3이 포함되어 있는지 인덱스 2(arr.length - 1) 부터 확인한다.
arr.include(3, -1); // true

// indexOf 메서드도 있지만 반환값이 -1임을 확인해야하고 NaN이 포함되었는지 확인할 수 없다.
[NaN].indexOf(NaN) !== -1; // false
[NaN].includes(NaN); // true

// 27.8.14 Array.prototype.flat (ES10);
// 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]
[1, [2, [3, [4]]]].flat(); // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(1); // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(2); // [1, 2, 3, [4]]
[1, [2, [3, [4]]]].flat().flat(); // [1, 2, 3, [4]]
[1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4, 5]

```

# 27.9 배열 고차 함수
#HOF #고차함수 #불변성 #부수효과 #순수함수
고차함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.
고차함수는 외부 상태의 변경이나 가변 데이터를 피하고 **불변성**을 지향하는 함수형 프로그래밍에 기반을 두고있다.

함수형 프로그래밍은 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거** 하여 복잡성을 해결하고 **변수의 사용을 억제**하여 상태 변경을 피하려는 프로그래밍 패러다임이다.

함수형 프로그래밍은 결국 **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그래밍의 안정성을 높이려는 노력의 일환이라 할 수 있다.

```js
// 27.9.1 Array.prototype.sort
const fruits = ['Banana', 'Orange', 'Apple']
// 오름차순으로 정렬
fruits.sort();
// 원본 배열을 직접 변경한다.
console.log(fruits); // ['Apple', 'Banana', 'Orange']

const fruits = ['바나나', '오렌지', '사과'];
// 오름차순으로 정렬
fruits.sort();
// 원본 배열을 직접 변경한다.
console.log(fruits); // ['바나나', '사과', '오렌지']

// 내림차순으로 정렬
const fruits = ['Banana', 'Orange', 'Apple']
fruits.sort();
fruits.reverse();

// 숫자 요소를 정렬할때 주의가 필요하다.
const points = [40, 100, 1, 5, 2, 25, 10];
points.sort();
// 원하는대로 정렬이 되지 않는다.
console.log(points); // [1, 10, 100, 2, 25, 40, 5]

// sort는 유니코드 코드 포인트의 순서를 따른다.
// sort는 일시적으로 문자열로 변환한 후 정렬하므로 배열 [2, 1]을 정렬해도 [1,2]로 된다.
['2', '1'].sort(); // ["1", "2"]
[2, 1].sort(); // [1, 2]

// 숫자 요소를 정렬할 때는 sort 메서드에 정렬 순서를 정의하는 비교 함수를 전달해야한다.
/*
 * 양수 or 음수 or 0을 반환해야한다.
 * 0 보다 작으면 비교 함수의 첫 번째 인수를 우선하여 정렬하고
 * 0 이면 정렬하지 않으며
 * 0 보다 크면 두 번째 인수를 우선하여 정렬한다.
 */
const points = [40, 100, 1, 5, 2, 25, 10];
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]
points.sort((a, b) => a + b);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

const todos = [
	{id: 4, content: 'JavaScript'},
	{id: 1, content: 'HTML'},
	{id: 2, content: 'CSS'}
]
function compare(key) {
	return (a, b) => (a[key] > b[key] ? 1 : (a[key] < b[key] ? -1 : 0));
}

todos.sort(compare('id'));
console.log(todos);
/*
[
	{id: 1, content: 'HTML'},
	{id: 2, content: 'CSS'}
	{id: 4, content: 'JavaScript'},
]
*/
/*
sort는 quicksort 알고리즘을 사용했었다.
quicksort 알고리즘은 동일한 값의 요소가 중복되어 있을 때 초기 순서와 변경 될 수 있는
불안정한 정렬 알고리즘으로 알려져 있다.
ECMAScript 2019(ES10)에서는 timsort 알고리즘을 사용하도록 변경되었다.
*/

// 267.9.2 Array.prototype.forEach
const numbers =[1, 2, 3];
const pows = [];

numbers.forEach(item => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
// 원본을 변경하지 않는다.
console.log(numbers); // [1, 2, 3]

// forEach의 반환값은 언제나 undefined다.
const result = [1, 2, 3].forEach(console.log);
console.log(result); // undefined

// 두 번째 인수로 forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있다.
class Numbers {
	numberArray = [];
	multiply(arr) {
		arr.forEach(function (item) {
			// TypeError
			this.numberArray.push(item * item);
		})
	}
	multiply2(arr) {
		arr.forEach(function (item) {
			this.numberArray.push(item * item);
		}, this) // forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달
	}
	multiply2(arr) {
		// 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.
		arr.forEach(item => item.numberArray.push(item * item))
	}
}

// forEach 메서드는 for문과는 달리 break, continue 문을 사용할 수 없다.
[1,2,3].forEach(item => {
	item(item > 1) break; // SyntaxError
})
[1,2,3].forEach(item => {
	item(item > 1) continue; // SyntaxError
})

// 희소 배열의 경우 존재하지 않는 요소는 순회 대상에서 제외된다.
// map, filter, reduce 모두 동일
const arr = [1, , 3];
arr.forEach(v => console.log(v)) // 1, 3

// 27.9.3 Array.prototype.map
// 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
// 원본은 변경되지 않는다.
const numbers = [1, 4, 9];
const roots = numbers.map(item => Math.sqrt(item));
console.log(roots); // [1, 2, 3]
console.log(numbers); // [1, 4, 9]

/*
map 메서드가 생성하여 반환하는 새로운 배열의 length 프로퍼티 값은
map 메서드를 호출한 배열의 length 프로퍼티 값과 반드시 일치한다.
즉 map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열은 1:1 매핑한다.
*/

// 27.9.4 Array.prototype.filter
// 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.
const numbers = [1,2,3,4,5];
const odds = numbers.filter(item => item % 2);

// 27.9.5 Array.prototype.reduce
const sum = [1,2,3,4].reduce((acc,cur,index,arr) => acc + cur, 0)
console.log(sum); // 10

// 평균 구하기
const values = [1, 2, 3, 4, 5, 6];
const average = values.reduce((acc, cur, i, { lnegth }) => {
	return i === length - 1 ? ( acc + cur ) / length : acc + cur;
},0);
// 최대값 구하기
const values = [1, 2, 3, 4, 5];
const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);
// Math.max 가 더 적합하다.
const max = Math.max(...values);
console.log(max); // 5

// 요수의 중복 횟수 구하기
const fruits = ['banana', 'apple', 'orange', 'orange', 'apple'];
const count = fruits.reduce((acc, cur) => {
	acc[cur] = (acc[cur] || 0) + 1;
	return acc;
},{});
console.log(count); // { banana: 1, apple: 2, orange: 2 }

// 중첩 배열 평탄화
const values = [1, [2, 3], 4, [5, 6]];
const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
console.log(flatten); // [1, 2, 3, 4, 5, 6]
// flat 메서드를 쓰자
values.flat(2);

// 중복 요소 제거
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
const result = values.reduce(
	(unique, val, i, _value) =>
		_values.indexOf(val) === i ? [...unique, val] : unique,
	[]
);
console.log(result); // [1,2,3,4,5,4]
// filter가 더 직관적이다.
const result = values.filter((val, i, _values) => _values.indexOf(val) === i);
// Set을 사용할 수 있다.
const result = [... new Set(values)];

// 이처럼 map, filter, some, every, find 같은
// 모든 배열의 고차함수는 reduce 메서드로 구현할 수 있다.

// reduce 메서드를 호출할 때는 언제나 초기값을 전달하는 것이 안전하다
const sum = [].reduce((acc, cur) => acc + cur); // TypeError
const sum = [].reduce((acc, cur) => acc + cur, 0);

// 27.9.6 Array.prototype.some
// 콜백 함수의 반환값이 단 한번이라도 참이면 true 아니면 false
[5, 10, 15].some(item => item > 10); // true
[5, 10, 15].some(item => item < 0); // false
[].some(item => item > 3); // 빈 배열인 경우 언제나 false를 반환한다.

// 27.9.7 Array.prototype.every
// 콜백 함수의 반환값이 모두 참이면 true 아니면 false
[5, 10, 15].some(item => item > 3); // true
[5, 10, 15].some(item => item > 10); // false
[].some(item => > 3); // 빈 배열인 경우 언제나 true 를 반환한다.

// 27.9.8 Array.find
const users = [
	{id: 1, name: 'Lee'},
	{id: 2 name: 'Kim'},
	{id: 2, name: 'Choi'},
	{id: 3, name: 'Pack'}
];
users.find(user => user.id === 2); // {id: 2, name: 'Kim'}

// 27.9.9 Array.prototype.findIndex
users.findIndex(user => user.id === 2); // 1

// 27.9.10 Array.prototype.flatMap (ES10)
// flatMap 메서드는 map 메서드를 통해 성성된 새로운 배열을 평탄화한다.
// map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.
// 단 1단계만 평탄화 한다.
const arr = ['hello', 'world'];
app.map(x => x.split('')).flat();
// ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'd']
arr.flatMap(x => x.split(''));
// ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'd']
```

# 알고 가기
---
- [ ] 배열 고차함수 3가지만 설명해주세요.
