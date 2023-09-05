# 24. 클로저

> 클로저는 난해하기로 유명한 자바스크립트의 개념 중 하나이다.
> 클로저는 자바스크립트 고유의 개념이 아니다. 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성이다.

> MDN 에서는 클로저에 대해 다음과 같이 정의하고 있다.

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

```javascript
const x = 1;

function outerFunc() {
    const x = 10;
    
    function innerFunc() {
        console.log(x); // 10
    }
    
    innerFunc();
}

outerFunc();
```

> outerFunc 함수 내부에서 중첩 함수 innerFunc 가 정의되고 호출되었다.
> 중첩 함수 innerFunc 상위 스코프는 외부 함수 outerFunc의 스코프다.
> 따라서 중첩 함수 innerFunc 내부에서 자신을 포함하고 있는 외부 함수 outerFunc 의 x 변수에 접근할 수 있다.

> 만약 innerFunc 함수가 outerFunc 함수 내부에서 정의된 중첩 함수가 아니라면 innerFunc 함수를 outerFunc 함수 내부에서 호출한다 하더라도 outerFunc 함수 변수에 접근할 수 없다.

> 이 같은 현상이 발생하는 이유는 자바스크립트 렉시컬 스코프를 따르는 프로그래밍 언어이기 떄문이다.

## 24.1 렉시컬 스코프

> 자바스크립트 엔진은 함수를 어디서 호출하는지가 아니라 어디에 정의했는지에 따라 상위 스코프를 결정한다.
> 이를 렉시컬 스코프라 한다.

```javascript
const x = 1;

function foo() {
    const x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo(); // 1
bar(); // 1
```

> foo 함수와 bar 함수의 상위 스코프는 전역이다.
> 함수를 어디서 호출하는지는 함수의 상위 스코프 결정에 아무런 영향을 주지 못한다.

> 즉 함수의 상위 스코프는 함수를 정의한 위치에 의해 정적으로 결정되고 변하지 않는다.

> 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에
> 함수가 정의된 환경에 의해 결정된다. 이것이 바로 렉시컬 스코프다.

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

> 함수가 정의된 환경과 호출되는 환경은 다를 수 있다. 렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과는 상관없이 자신이 정의된 환경, 즉 상위 스코프를 기억해야 한다.
> 이를 위해 함수는 자신의 내부 슬롯 [[Environment]] 에 자신이 정의된 환경 상위 스코프의 참조를 저장한다.

> 함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯에 저장한다.
> 이때 자신의 내부 슬롯에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.

> 함수 정의가 평가되어 함수 객체를 생성하는 시점은 함수가 정의된 환경, 상위 함수가 평가 또는 실행되고 있는 시점이며, 현재 실행 중인 실행 컨텍스트는 상위 함수의 실행 컨텍스트이기 떄문이다.

> 함수 객체의 내부 슬롯 [[Environment]] 에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프다.
> 또한 자신이 호출되었을 때 생성된 함수 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조'에 저장될 참조값이다.
> 함수 객체는 내부 슬롯 [[Environment]] 에 저장된 렉시컬 환경의 참조 삼위 스코프를 자신이 존재하는 한 기억한다.

```javascript
const x = 1;

function foo() {
    const x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo();
bar();
```

> foo 함수와 bar 함수는 모두 전역에서 함수 선언문으로 정의되었다. 따라서 foo 함수와 bar 함수는 모두 전역 코드가 평가되는 시점에 평가되어
> 함수 객체를 생성하고 전역 객체 window 메서드가 된다. 이때 생성된 함수 객체의 내부 슬롯 [[Environment]] 에는 함수 정의가 평가된 시점, 즉 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장된다.

## 24.3 클로저와 렉시컬 환경

```javascript
const x = 1;

function outer() {
    const x = 10;
    const inner = function () {console.log(x)};
    return inner;
}

const innerFunc = outer();
innerFunc(); // 10
```

> outer 함수를 호출하면 outer 함수는 중첩 함수 inner 를 반환하고 생명 주기를 마감한다. 즉 outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다.
> 이때 outer 함수의 지역 변수 x 와 변수 값 10을 저장하고 있던 outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 변수 x 또한 생명 주기를 마감한다. 따라서 outer 함수의 지역 변수 x는 더는 유효하지 않게 되어 x 변수에 접근할 수 있는 방법은 달리 없어 보인다.

> 그러나 위 코드의 실행 결과는 outer 함수의 지역 변수 x 값인 10이다. 이미 생명 주기가 종료되어 실행 컨텍스트 스택에서 종료된 outer 함수의 지역 변수가 다시 부활하듯이 작동하고있다.

> 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저 라고 한다.

> 자바스크립트의 모든 함수는 자신의 상위 스코프를 기억한다. 모든 함수가 기억하는 상위 스코프는 함수를 어디서 호출하든 상관없이 유지된다. 따라서 함수를 어디서 호출하든 상관없이 함수는 언제나 자신이 기억하는 상위 스코프의 식별자를 참조할 수 있으며 식별자에 바인딩된 값을 변경할 수도 있다.

> 위 예제에서 outer 함수가 평가되어 함수 객체를 생성할 때 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의 [[Environment]] 내부 슬롯에 상위 스코프로서 저장한다.

> outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고 앞서 outer 함수 객체의 [[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 할당한다.

> outer 함수의 실행이 종료되면 inner 함수를 반환하면서 outer 함수의 생명 주기가 종료된다. 이때 outer 함수의 실행 컨텍스트는 실행 컨텍스트에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.

> outer 함수가 반환한 inner 함수를 호출하면 inner 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다. 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당된다.

> 중첩 함수 inner 는 외부 함수 outer 보다 오래 생존했다. 외부 함수보다 오래 생존한 중첩 함수는 외부 함수의 생존 여부와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다. 

```javascript
// 클로저가 아니다.
function foo() {
    const x = 1;
    const y = 2;
    
    function bar() {
        const z = 3;
        
        debugger;

        console.log(z);
    }
    
    return bar;
}

const bar = foo();
bar();
```

> 클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.

> 클로저에 의해 참조되는 상위 스코프의 변수를 자유 변수라고 부른다. 클로저란 함수가 자유 변수에 대해 닫혀있다. 라는 의미다. 쉽게 말하면 자유 변수에 묶여있는 함수 라고 할 수 있다.

> 이론적으로 클로저는 상위 스코프를 기억해야 하므로 불필요한 메모리의 점유를 걱정할 수도 있겠다. 하지만 모던 자바스크립트 엔진은 최적화가 잘 되어 있어서 클로저가 참조하고 있지 않는 식별자는 기억하지 않는다.

## 24.4 클로저의 활용

> 클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다. 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```javascript
let num = 0;

const increase = function () {
    return ++num;
}
```

해당 함수에서 변경해야 하는 점은 다음과 같다.

1. 카운트 상태는 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 카운트 상태는 increase 함수만이 변경할 수 있어야 한다.

> 하지만 카운트 상태는 전역 변수를 통해 관리되고 있기 때문에 누구나 접근이 가능하고 변경도 가능하다.

```javascript
const increase = function () {
    let num = 0;
    
    return num++;
};

console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

> 카운트 상태를 안전하게 변경하고 유지하기 위한 전역 변수 num 을 지역 변수로 변경하여 의도치 않은 상태 변경을 방지하였지만 0으로 초기화가 되기 때문에 항상 결과값이 같다.

```javascript
const increase = (function () {
    let num = 0;
    
    return function () {
        return ++num;
    }
})();
```

> 위 코드가 실행되면 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 increase 변수에 할당된다.

> 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

```javascript
const counter = (function () {
    let num = 0;
    
    return {
        increase() {
            return ++num;
        },
        decrease() {
            return num > 0 ? --num : 0;
        }
    }
})
```

> 위 예제의 증가 감소 메서드의 상위 스코프는 메서드가 평가되는 시점에 실행중인 실행 컨텍스트인 즉시 실행 함수 컨텍스트의 렉시컬 환경이다.

```javascript

function makeCounter(aux) {
    
    let counter = 0;
    
    return function () {
        
        counter = aux(counter);
        return counter;
    }
}

// 보조 함수
function increase(n) {
    return ++n;
}

function decrease(n) {
    return --n;
}

const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

> makeCounter 함수는 보조 함수를 인자를 전달받고 함수를 반환하는 고차 함수다. makeCounter 함수가 반환하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 makeCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다.

> makeCounter 함수는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있지만 makeCount 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다.

> 위 예제에서 전역 변수 increaser 와 decreaser 에 할당한 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter 를 공유하지 않아 카운터의 증감이 연동되지 않는다.

> 따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다.

```javascript
const counter = (function () {
    let counter = 0;
    
    return function (aux) {
        counter = aux(counter);
        
        return counter;
    }
})();

const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // 1
console.log(decreaser()); // 0
```

## 24.5 캡슐화와 정보 은닉

> 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

> 정보 은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고 결합도를 낮추는 효과가 있다.

> 자바스크립트는 public, private, protected 같은 접근 제한자를 제공하지 않는다. 자바스크립트의 객체의 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어 있다.

```javascript

function Person(name, age) {
    this.name = name;
    let _age = age;
    
    this.sayHi = function () {
        console.log(`Hello world ${this.name} ${_age}`);
    }
}

const me = new Person('Lee', 20);
me.sayHi(); // Hello world Lee 20
console.log(me.name, me._age); // Lee undefined


```

> name 프로퍼티는 외부로 공개되어 있어서 자유롭개 참조하거나 변경할 수 있다. age 같은 경우는 private 하다.


## 24.6 자주 발생하는 실수

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = function () { return i; };
}

for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
}
```

> for 코드 문의 코드 블록 내에서 함수가 funcs 배열의 요소로 추가된다. 그리고 두 번째 for 문의 코드 블록 내에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출한다.
> 배열의 요소로 추가한 3개의 함수가 0, 1, 2 를 반환할 것이라 기대했다면 결과는 그렇지 않다.

> for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수다.
> 전역 변수 i에는 0, 1, 2 가 순차적으로 할당된다. 따라서 funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참조하여 i의 값 3이 출력된다.

```javascript
var funcs = [];

for (var i = 0; i < 3; i++) {
    funcs[i] = (function (id) { // 1
        return function () {
            return id;
        }
    })(i);
}
```

> 1 에서 즉시 실행 함수는 전역 변수 i에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료된다. 

> 위 예제는 함수 레벨 스코프를 가진 var 일 경우에 나타나는 오류이므로 let 을 사용한다면 이 같은 번거로움 없이 해결 가능하다.

