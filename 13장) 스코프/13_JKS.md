# 13. 스코프

## 13.1 스코프란?

> 함수의 매개변수는 함수 몸체 내부에서만 참조할 수 있고 함수 몸체 외부에서는 참조할 수 없다.

> 이것은 매개변수를 참조할 수 있는 유효범위, 즉 매개변수의 스코프가 함수 몸체 내부로 한정되기 떄문이다.

---

```javascript
function add(x, y) {
    console.log(x, y); // 2, 5
    return x + y;
}

add(2, 5);
```

> 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는
> 유효 범위가 결정된다. 이를 스코프라 한다. 즉 스코프는 식별자가 유효한 범위를 말한다.

```javascript
var x = 'global';

function foo() {
    var x = 'local';
    console.log(x); // local
};

foo();

console.log(x); // global
```

> 코드의 가장 바깥 영역과 foo 함수 내부에 같은 이름을 갖는 x 변수를 선언
> foo 함수 내에서 어떤 변수를 참조해야 할 것인지를 결정해야 한다.
> 이를 식별자 결정 이라 한다. 자바스크립트 엔진은 스코프를 통해 어떤 변수를 참조해야 할 것인지 결정한다.

> 스코프란 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙이라고도 할 수 있다.

### 코드의 문맥과 환경

> 코드가 어디서 실행되며 주변에 어떤 코드가 있는지를 렉시컬 환경이라고 부른다.
> 즉 코드의 문맥은 렉시컬 환경으로 이뤄진다. 이를 구현한 것이 실행 컨텍스트이며
> 모든 코드는 실행 컨텍스트에서 평가되고 실행된다.
> 스코프는 실행 컨텍스트와 깊은 관련이 있다.

> 위 예제에서 코드 가장 바깥쪽에서 선언된 'global' 은 어디서든 참조할 수 있다.

> 하지만 foo 함수 내부에서 선언된 x 변수는 foo 함수 내부에서만 참조할 수 있고 foo 함수 외부에서는 참조할 수 없다.

> 이때 두 개의 x 변수는 이름이 동일하지만 자신이 유효한 범위, 즉 스코프가 다른 별개의 변수다.

> 식별자는 어떤 값을 구별할 수 있어야 하므로 유일 해야한다. 따라서 식별자인 변수 이름은 중복될 수 없다.

> 프로그래밍 언어에서는 스코프를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를
> 사용할 수 있게 한다. 스코프 내에서 식별자는 유일해야 하지만
> 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다.

## 13.2 스코프의 종류

> 코드는 전역과 지역으로 구분할 수 있다.

| 구분  | 설명           | 스코프    | 변수    |
|-----|--------------|--------|-------|
| 전역  | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역  | 함수 몸체 내부     | 지역 스코프 | 지역 변수 |

> 이때 변수는 자신이 선언된 위치에 의해 자신이 유요한 범위인 스코프가 결정된다.

```javascript
var x = 1;
var y = 2;

function foo() {
    var x1 = '1'; // local
    var y1 = '2'; // local
    
    function foo2() {
        var x2 = '11'; // local
        var y2 = '22'; // local

        console.log(x1, y1);
    }
    
    foo2();
    
}

foo();
```

> 전역이란 코드의 가장 바깥 영역을 말한다.

### 13.2.2 지역과 지역 스코프

> 지역이란 함수 몸체 내부를 말한다. 지역은 지역 스코프를 만든다.
> 지역 변수는 자신이 선언된 지역과 하위 지역에서만 참조할 수 있다.

> 지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

## 13.3 스코프 체인

> 함수는 전역에서 정의할 수도 있고 함수 몸체 내부에서 정의할 수도 있다.
> 함수 몸체 내부에서 함수가 정의된 것을 함수의 중첩이라 한다.

> 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다.

> 중첩 함수의 지역 스코프는 중첩 함수를 포함하는 외부 함수의 지역 스코프와 계층적 구조를 갖는다.

> 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하여
> 선언된 변수를 검색한다.

### 13.3.2 스코프 체인에 의한 함수 검색

```javascript
function foo() {
    console.log('global');
}

function bar() {
    function foo() {
        console.log('local');
    }
}

foo(); // 'global'
```

> 함수 선언문으로 함수를 정의하면 런타임 이전에 함수 객체가 먼저 생성된다.
> foo 함수를 호출하면 자바스크립트 엔진은 함수를 호출하기 위해 먼저 함수를 가리키는 식별자 foo 를 검색한다.

## 13.4 함수 레벨 스코프

> 지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만든다고 했다.
> 이는 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성된다는 의미다.

> C나 자바 등을 비롯한 대부분의 프로그래밍 언어는 함수 몸체만이 아니라 모든 코드 블록이 지역 스코프를 만든다.
> 이러한 특성을 블록 레벨 스코프라 한다.

> 하지만 var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.

> 이러한 특성을 함수 렙레 스코프라 한다.

```javascript
var x = 1;

if (true) {
    var x = 10;
}

console.log(x); // 10
```

> 전역 변수 x가 선언되었고 if 문의 코드 블록 내에서도 x 변수가 선언되었다.
> if 문의 코드 블록에서 선언된 x 변수는 전역 변수다.

```javascript
var i = 10;

for (var i = 0; i < 5; i++) {
    console.log(i); // 0 1 2 3 4
}

// 의도치 않게 변수의 값이 변경되었다.
console.log(i); // 5
```

> 블록 레벨 스코프를 지원하는 프로그래밍 언어에서는 for 문에서 반복을 위해 선언된 i 변수가
> for 문의 블록 내에서만 유효한 지역 변수다.

> 이 변수를 외부에서 사용할 일은 없기 때문이다.
> 하지만 var 키워드로 선언된 변수는 블록 레벨 스코프를 인정하지 않는다. i 변수는 전역 변수가 된다.

> let, const 키워드는 블록 레벨 스코프를 지원한다.

## 13.5 렉시컬 스코프

```javascript
var x = 1;

function foo() {
    var x = 10;
    bar();
}

function bar() {
    console.log(x);
}

foo();
bar();
```

> 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에
> 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다.
> 즉 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.

> 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의가 실행되어 생성된 함수 객체는 이렇게
> 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있다.

---

[] 렉시컬 스코프에 대해서 설명하시오
[] 스코프에 대해서 설명하시오.







