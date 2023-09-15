# 36. 디스트럭처링 할당

> 디스트럭처링 할당은 구조화된 배과 같은 이터러블 또는 객체를 destructuring 하여 
> 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

## 36.1 배열 디스트럭처링 할당

> ES5에서 구조화된 배열을 디스트럭처링하여 1개 이상의 변수에 할당하는 방법은 다음과 같다.

```javascript
var arr = [1,2,3];

var one = arr[0];
var two = arr[1];
var three = arr[2];
```

```javascript
// ES6

const arr = [1,2,3];

const [one, two, thee] = arr;
```

> 우변에 이터러블을 할당하지 않으면 에러가 발생한다.

> 배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소를 사용할 수 있다.

```javascript
const [x, ...y] = [1, 2, 3];

console.log(x, y); // 1 , [2,3]
```

## 36.2 객체 디스트럭처링 할당

> ES5 에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.

```javascript
// ES5
var user = { firstName: 'first', lastName: 'Lee' };

var firstName = user.firstName;
var lastName = user.lastName;
```
> ES6 객체 디스트럭처링 할당은 객체의 각프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.
> 할당 기준은 프로퍼티 키다.

```javascript
var user = { firstName: 'first', lastName: 'Lee' };

const { lastName, firstName } = user;

console.log(lastName, firstName);
```

> 우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러가 발생한다.

> 객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.

```javascript
const { firstName = '1', lastName} = { lastName: 'Lee'};

console.log(firstName, lastName); // 1 Lee
```

---

생략