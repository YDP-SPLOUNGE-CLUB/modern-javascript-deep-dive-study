# 20. strict mode

## 20.1 strict mode 란?

```javascript
function foo() {
    x = 10;
}

foo();

console.log(x); // ?
```

> foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당했다.
> 스코프 체인을 통해 검색하기 시작한다.

> 자바스크립트 엔진은 먼저 foo 함수의 스코프에서 x 변수의 선언을 검색한다. foo 함수의 스코프에는 x 변수의 선언이 없으므로
> 검색에 실패할 것이고, 자바스크립트 엔진은 x 변수를 검색하기 위해 foo 함수 컨텍스트의 상위 스코프에서 x 변수의 선언을 검색한다.

> 전역 스코프에도 x 변수의 선언이 존재하지 않기 때문에 ReferenceError 를 발생시킬 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생산한다.
> 이러한 현상을 암묵적 전역 이라 한다.

> ES5 부터 strict mode 가 추가되었다. 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여
> 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

> ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있다.
> strict mode 보다 ESLint 를 사용하는 편을 권장한다.

## 20.2 strict mode 의 적용

> 전역의 선두 또는 함수 몸체의 선두에 use strict 를 추가한다.

```javascript
'use strict';

function foo() {
    x = 10;
}

foo();
```

> 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode 가 적용된다.

## 20.3 전역에 strict mode 를 적용하는 것은 피하자

> strict 스크립트와 non-strict 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다.

## 20.4 함수 단위로 strict mode 를 적용하는 것도 피하자

> 함수 단위로 strict mode 를 적용할 수 있지만 모든 함수에 일일히 strict mode를 걸거나 하나씩 거는건 부적절하다.

## 20.5 strict mode 가 발생시키는 에러

### 20.5.1 암묵적 전역

```javascript
(function (){
    'use strict';
    x = 1;
    console.log(x); // ReferenceError
})();
```

### 20.5.2 변수, 함수, 매개변수의 삭제

```javascript
(function (){
    'use strict';
    
    var x = 1;
    delete x; // syntaxError
    function foo(a) {
        delete a; // syntaxError
    }
    
    delete foo; // syntaxError
})();
```

### 20.5.3 매개변수 이름의 중복


```javascript
(function (){
    'use strict';
    
    // syntaxError
    function foo(x, x) {
        return x + x;
    }

    console.log(foo(1,2));
})()
```

### 20.5.4 with 문의 사용

> with 문을 사용하면 syntaxError 가 발생한다.
> with 문은 전달된 객체를 스코프 체인에 추가한다. with 문은 동일한 객체의 프로퍼티를
> 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 가독성이 좋지 않아진다.

```javascript
(function () {
    'use strict';
    
    with({x: 1}) {
        console.log(x);
    }
})()
```

### 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

```javascript
(function () {
    'use strict';
    
    function foo() {
        console.log(this); // undefined
    }
    foo();
    
    function Foo() {
        console.log(this); // Foo
    }
})()
```

### 20.6.2 arguments 객체

> strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```javascript
(function (a) {
    'use strict';
    
    a = 2;

    console.log(arguments); // {0: 1, length: 1}
})(1);
```
