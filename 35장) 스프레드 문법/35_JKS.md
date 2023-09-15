# 35 스프레드 문법

> ES6에서 도입된 스프레드 문법 ... 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

> 스프레드 문법을 사용할 수 있는 대상은 이터러블에 한정된다.

> 스프레드 문법의 결과물은 사용할 수 없고 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용 가능하다.

1. 함수 호출문의 인수 목록
2. 배열 리터럴의 요소 목록
3. 객체 리터럴의 프로퍼티 목록

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

```javascript
const arr = [1, 2, 3];

const max = Math.max(arr); // NaN
```

> 이 문제를 해결하기 위해 배열을 펼쳐서 요소들을 개별적인 값들의 목록으로 만든 후 Math.max 메서드의 인수로 전달해야 한다.
> 즉 [1,2,3] 을 1,2,3 으로 펼쳐서 Math.max 메서드 인수로 전달해야 한다.

> 스프레드 문법이 제공되기 이전에는 배열을 펼쳐서 요소들의 목록을 함수의 인수로 전달하고 싶은 경우
> Function.prototype.apply 를 사용했다.

```javascript
var arr = [1, 2, 3];

var max = Math.max.apply(null, arr);

Math.max(...arr);
```

> 스프레드 문법은 앞에서 살펴본 Rest 파라미터와 형태가 동일하여 혼동할 수 있다.
> Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 일므 앞에 ... 을 붙이는 것이다.
> 스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다.

## 35.2 배열 리터럴 내부에서 사용하는 경우

### 35.2.1 concat

> ES5에서 2개의 배열을 1개의 배열로 결합하고 싶은 경우 배열 리터럴만으로 해결할 수 없고 concat 메서드를 사용해야 한다.

```javascript
// ES5
var arr = [1,2].concat([3,4]);
console.log(arr); // [1,2,3,4]

// ES6
const arr2 = [...[1,2], ...[3,4]];
console.log(arr2); // [1,2,3,4]
```

### 35.2.2 splice

> ES5 에서 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 splice 메서드를 이용한다.

```javascript
var arr1= [1,4];
var arr2=[ 2,3];

arr1.splice(1, 0, arr2);

console.log(arr1); // [1,[2,3],4]
```

> 위 예제의 경우 Function.prototype.apply 메서드를 사용하여 해결 가능하다.

```javascript
var arr1 = [1,4];
var arr2 = [2,3];

Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
```

> 스프레드 문법을 사용하면 가독성 좋고 간결하게 표현 가능하다.

```javascript
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1,2,3,4]
```

### 35.2.3 배열 복사

```javascript
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2];
console.log(copy === origin); // false
```

### 35.2.4 이터러블을 배열로 변환

> ES5에서 이터러블을 배열로 변환하려면 Function.prototype.apply or call 메서드를 사용하여 slice 메서드를 호출해야 한다.

```javascript
function sum() {
    var args = Array.prototype.slice.call(arguments);
    
    return args.reduce(function (pre, cur) {
        return pre + cur
    }, 0);
}
```

> 스프레드 문법을 사용하면 좀 더 간편하게 이터러블을 배열로 변환 가능하다.

```javascript
function sum() {
    return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

> Rest 프로퍼티와 함께 스프레드 프로퍼티를 활용하면 객체 리터럴 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.
> 스프레드 문법은 이터러블 이어야 하지만 스프레드 프로퍼티 제안은 일반 객체 대상으로도 사용을 허용한다.

```javascript
const obj = { x:1, y:2 }
const copy = { ...obj };

console.log(copy); // { x:1, y:2 }
console.log(obj === copy); // false

const merged = {x:1 ,y: 2, ...{a:3, b:4}};
console.log(merged); // { x:1, y:2, a:3, b:4 }
```

---

생략
