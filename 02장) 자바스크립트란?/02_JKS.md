## 2.1 ~ 2 자바스크립트의 탄생, 표준화

> 1995년 넷스케이프 커뮤니케이션즈는 웹페이지의 보조적인 기능을 수행하기 위해 브라우저에서 동작하는
> 경량 프로그래밍 언어를 도입하기로 결정한다. 이것이 브렌던 아이크의 자바스크립트이다.

> 자바스크립트의 탄생후 얼마 지나지 않아 파생 버전 "JScript" 가 출시되었다.

> 마이크로소프트는 자바스크립트의 파생 버전의 "JScript"를 인터넷 익스플로어 3.0 에 탑재하였다.
> 문제는 JScript와 자바스크립트가 표준화되지 못하고 적당히 호환되었다.

> 자사 브라우저의 시장 점유율을 올리기 위해서 자사 브루아저만 작동하는 기능을 추가하였다.
> 그로인해 크로스 브라우징 이슈가 발생하였고 웹페이지의 개발비용이 증가되었다.

> 모든 브라우저에서 정상적으로 동작하는 표준화된 자바스크립트가 필요해짐에 따라
> 1996년 자바스크립트는 컴퓨터 시스템의 표준을 관리하는 비영리 표준화 기구
> **"ECMA" 인터내셔널**에 자바스크립트의 표준화를 요청하였다.

> 1997년 ES1 사양이 완성, 상표권 문제로 ECMAScript 로 명명되었다.

> 1999년 ES3가 공개되었다.

> 2009년 ES5 출시로 HTML5 와 함께 출현한 표준 사양이다.

> 2015년 ECMAScript6는 let/const 키워드, 화살표 함수, 클래스, 모듈 등과 같이
> 범용 프로그래밍 언어로서 갖춰야 할 기능들을 대거 도입하는 큰 변화가 있었다.
> ES6 이후 버전업은 비교적 작은 기능을 추가하는 수준으로 매년 공개할 것이라 예고하였다.

| 버전   | 출시 연도 | 특징                                                                                                                        |                             
|------|-------|---------------------------------------------------------------------------------------------------------------------------|
| ES1  | 1997  | 초판                                                                                                                        |
| ES2  | 1998  | ISO/IEC 16262 국제 표준과 동일한 규격 적용                                                                                            |
| ES3  | 1999  | 정규 표현식, try catch                                                                                                         |
| ES5  | 2009  | HTML 와 함께 출현한 표준안, JSON, strict mode, 접근자 프로퍼티, 프로퍼티 어트리뷰트 제어, 향상된 배열, 조작 기능 (forEach map, filter, reduce, some, every)   |
| ES6  | 2015  | let/const, 화살표, 템플릿 리터널, 디스트럭처링 할당, 스프레드, rest 파라미터, 심벌, 프로미스, Map/Set 이터러블 for ... of, 제너레이터, Proxy, 모듈 import/export    |
| ES7  | 2016  | 지수 연산자, Array, prototype, includes, String.prototype.includes                                                             |
| ES8  | 2017  | async/await Object 정적 메서드(Object.values, Object.entries, Object.getOwnPropertyDescriptors)                                |
| ES9  | 2018  | Object rest/spread 프로퍼티, Promise.prototype, finally, async generator, for await ... of                                    |
| ES10 | 2019  | Object.formEntries, Array.prototype.flat, Array.prototype.flatMap, optional catch binding                                 |
| ES11 | 2020  | String.prototype.matchAll, BigInt, globalThis, Promise.allSettled, null 병합 연산자, 옵셔널 체이닝 연산자, for ... in enumeration order |

## 생각보다 모르는게 많고 최근에 생긴것이 많다.

prototype 을 세세하게 사용해본적이 없었는데 많은 기능들을 제공하고 있었네요.

2020년 이후의 ECMAScript 에는 무슨 기능이 추가되었을까요?

| 버전   | 출시 연도 | 특징                                                                                                                                            |                             
|------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| ES12 | 2021  | 논리 할당 연산자 추가 &&=, ??=, 숫자 Separator 지원 1_000, Promise.any & AggregateError 추가, String.prototype.replaceAll, WeakRefs, FinallzationRegistry 객체 |

```javascript
// 논리 할당 연산자 추가
x ||= y;
x &&= y;
x ??= y;
x || (x = y);

// Number Separators
let fee = 123_00; // 123
let fee = 12_300; // 12,300

// Promise.any and AggregateError
Promise.any([
    fetch('https://...').then(() => '1'),
    fetch('https://...').then(() => '2'),
    fetch('https://...').then(() => '3'),
]).then((first) => {
    // 아무 promise가 resolve 된다면
    console.log(first)
}).catch((error) => {
    // 모든 promise가 reject 시 AggregateError
    console.log(error);
});

// String.prototype.replaceAll
const p = 'hello world';
const reg = /HELLO/ig;

console.log(p.replaceAll('hello', 'bye')); // bye world;
console.log(p.replaceAll(reg, 'HALO')); // HALO world;

// WeakRef
// WeakRef는 참조된 객체가 가비지 콜렉터의 대상이 될 때, 그 내부의 객체가 사라집니다.

let user = {id: 1};
const ref = new WeakRef(user);

ref.deref(); // {id: 1}

user = null; // 가비지 콜렉션

ref.deref(); // undefined

// FinalizationRegistry
// Map 내에서 WeakRef를 사용할 경우 계속해서 메모리를 참조
// 이럴 경우 FinalizationRegistry를 사용할 경우 등록된 객체가
// 가비지 컬렉션의 대상이 될 때 함께 등록된 값을 이벤트로 호출

class UserFinder {
    #cachedUsers = new Map()
    #registry

    constructor() {
        // 가비지 컬렉션 발동 시 캐시user 안의 weakRef를 삭제
        this.#registry = new FinalizationRegistry((id) => {
            this.#cachedUsers.delete(id)
        })
    }
}
```


| 버전   | 출시 연도 | 특징                                                                                                                                            |                             
|------|-------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| ES13 | 2022  | 퍼블릭 필드 및 정적 필드 선언 방식 개선, 프라이빗 접근 제어자 추가, 정적 초기화 블록 추가, in 연산자, 최상위 레벨 await 모듈 호출 가능, at, error.prototype.cause, Object.hasOwn(), 정규표현식 플래그 d |


```javascript
// 클래스 필드 개선
// 접근 제어자가 부실한 JS 태생 한계를 극복하고,클래스 선언과 관련된 
// 코드들의 가독성과 지역성을 높이기 위해 개발된 기능

class OldClass {
    constructor() {
        // this 를 사용하여 퍼블릭 필드를 설정
        this.publicField = 0;
        
        // 프라이빗 필드를 지원하지 않았던 과거의 프라이빗 필드 작성 방법
        this._privateField = 'private';
    }
    
    // 메서드 생성
    publicMethod() {}
    
    // 프라이빗 메서드 처럼 선언하였지만 퍼블릭하게 사용 가능
    _privateMethod() {}
}

// 정적 필드 메서드 설정을 위해서는 외부에서 설정
OldClass.staticField = '정적 필드';
OldClass.staticMethod = function () {}

class NewClass {
    // 클래스 내부에 바로 퍼블릭 필드 선언 가능
    publicField = 0;
    publicMethod() {};
    
    // 변수, 메서드 앞에 # 을 붙이면 프라이빗하게 설정이 됩니다.
    #privateField = 'private';
    #privateMethod() {};
    
    // getter / setter 또한 프라이빗하게 사용 가능
    get #privateGetter() {};
    set #privateSetter(value) {};
    
    // 정적 필드 또한 클래스 내부에 작성 가능
    static staticField = '정적 필드';
    static staticMethod() {};
    
    // 조합도 가능
    static #staticPrivateField = '정적 프라이빗';
    static get #staticPrivateGetter() {};
}


// ----------- //
// await모듈의 Top-Level 사용 가능

const response = await fetch('https://example.com');
const text = await response.text();
console.log(text);
// -------------------------------- //
// in연산자 를 통한 개인 슬롯 검사 기능 추가

// in 사용을 안하였을 경우
class ClassWithPrivateSlot {
    // 프라이빗 필드 접근을 할 경우에 try catch 이용
    #privateSlot = true;
    static hasPrivateSlot(obj) {
        try {
            obj.#privateSlot;
            return true;
        } catch {
            return false;
        }
    }
}

// in 연산자를 사용할 겨우
class ClassWithPrivateSlot {
    #privateSlot = true;
    static hasPrivateSlot(obj) {
        return #privateSlot in obj;
    }
}

const obj1 = new ClassWithPrivateSlot();
assert.equal(
    ClassWithPrivateSlot.hasPrivateSlot(obj1), true
)

const obj2 = {};
assert.equal(
    ClassWithPrivateSlot.hasPrivateSlot(obj2), false
);
// -------------------------- //

// error.cause
// Error의 오류 원인을 지정할 수 있습니다.
try {
    // ...
} catch (otherError) {
    throw new Error('...', {cause: otherError})
}

// ------ //
// .at 메서드 추가
// JS는 배열도 객체로 취급하여 음수 인덱싱을 할 수 없었는데. 
// at은 가능하도록 도와준다.

const arr = [0, 1, 2];

arr[-1] // undefiend

arr[arr.length - 1] // 2;

arr.at(-1) // 2;

// ------------- //

// Object.hasOwn(obj, propKey)
// Object.hasOwn(obj, propKey)객체에 키가 있는 자체obj (상속되지 않은) 
// 속성이 있는지 확인하는 안전한 방법을 제공합니다 .propKey

const proto = {
    protoProp: 'protoProp',
}

const obj = {
    __proto__: proto,
    objProp: 'objProp',
}

assert.equal('protoProp' in obj, true); // 상속된 속성을 감지

assert.equal(Object.hasOwn(obj, 'protoProp'), false); // 자체 속성만 검사
assert.equal(Object.hasOwn(proto, 'protoProp'), true); // 자체 속성만 검사

// --------- //

// RegExp 일치 인덱스?

// /d 플래그를 추가하면 플래그를 사용하여 
// 각 그룹의 시작 및 종료 인덱를 기록하는 일치 개체가 생성됨.

const matchObj = /(a+)(b+)/d.exec('aaaabb');

assert.equal(
    matchObj[1], 'aaaa'
);
assert.deepEqual(
    matchObj.indices[1], [0, 4] // (A)
);

assert.equal(
    matchObj[2], 'bb'
);
assert.deepEqual(
    matchObj.indices[2], [4, 6] // (B)
);

```

| 버전   | 출시 연도 | 특징                                                                                       |                             
|------|-------|------------------------------------------------------------------------------------------|
| ES14 | 2023  | Array Method, toSorted, toReversed, with, findLast, findLastIndex, toSpliced, Hashbang, WeakMap keys에 심벌 적용 가능 |

```javascript
// 기존의 find, findIndex 의 경우 배열의 처음부터 탐색을 하는데
// 이번 ECMAScript 버전에서는 뒤에서부터 찾는 기능이 추가되었습니다.

const isEven = (number) => number % 2 === 0;
const numbers = [1,2,3,4];

console.log(numbers.find(isEven)); // 2
console.log(numbers.findIndex(isEven)); // 1

console.log(numbers.findLast(isEven)); // 4
console.log(numbers.findLastIndex(isEven)); // 3


// --------------------- //

// Hashbang 문법?

// #!/usr/bin/env/node

// --------------------- //

// WeakMap keys에 Symbol 적용 가능

/**
 * Symbol : Object의 key로 사용되며 유일한 식별자를 만들 수 있습니다. Symbol의 description이 같아도 서로 다른 Symbol로 인식됩니다. String과 함께 Object의 key로 활용될 수 있는 유이한 타입입니다.
 * WeakMap : ES2015에서 추가된 객체로 key로 사용하는 값이 반드시 객체여야 합니다. key로 사용한 객체가 Garbage Collection 대상이 되기 때문에 key에 대응하는 Value가 사라질 수 있습니다. 부가 데이터를 저장하거나 값을 캐싱할 때 활용할 수 있습니다.
 */

const weakMap = new WeakMap();
const symbolKey = Symbol("write description here");
weakMap.set(symbolKey, "Symbol Key for WeakMap");

console.log(weakMap.get(symbolKey)); // Symbol Key for WeakMap

/**
 * Symbol을 key로 사용할 수 있게 되면 Record나 Tuple같은 불변 객체를 value로 저장했을 때,
 * 전역 스코프에서 유니크한 Symbol로 불변 객체에 접근할 수 있습니다. 
 * 이를 통해 프로그램 전체에서 동일한 값을 공유할 수 있습니다. 
 * 만약 Object를 key로 사용하는 경우에는 GC에 의해서 key와 함께 불변 객체가 사라질 위험이 있기 때문에
 * Symbol의 이점이 뚜렷할 것 같습니다.
 * 
 * 주의점 
 * Registered symbols (등록된 심볼) : 전역 심볼 레지스트리에 이미 등록되어 있는 값으로, WeakMap에서 key로 사용할 수 없습니다.
 * Well-Known symbols (잘 알려진 심볼) : 전역 심볼 레지스트리에 등록되어 있지 않기 때문에 WeakMap에서 key로 사용할 수 있습니다. EX) Symbol.iterator, Symbol.hasInstance 등
 */

```

와.. 길다..

## 2.3 자바스크립트 성장의 역사

> 초기에는 웹페이지의 보조적인 기능을 수행하기 위해 한정적인 용도로 사용되었다.

> 비동기 방식으로 데이터를 교환할 수 있는 통신 기능 **Ajax** 가 XMLHttpRequest 라는 이름으로 등장하였다.
> 기존의 모든 웹페이지 전체를 재 렌더링 하던 방식을 필요한 데이터만 전송받아 변경하게 되었다.

> 2006년 jQuery 등장으로 DOM 컨트롤이 쉽게 제어가 가능해지고 크로스 브라우징 문제도 어느 정도 해결되었다.

> V8 자바스크립트 엔진의 등장으로 더욱 빠르게 동작하게 되어 데스크톱 애플리케이션과 유사한 UX를 제공할 수 있는 웹 어플리케이션 프로그래밍 언어로 정착하게 되었다.
> 웹 서버에서 수행되던 로직들이 대거 클라이언트로 이동되었다.

> 2009년 라이언 달이 발표한 Node.js는 구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다.
> 브라우저의 자바스크립트 엔진에서만 동작하던 자바스크립트를 브라우저 이외의 환경에서도 동작할 수 있도록
> 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경이다.

> 프론트엔드와 백엔드 영역에서 자바스크립트를 사용할 수 있다는 동형성은 별도의 언어를 학습하기 위한 러닝커브를 줄일 수 있다.

> Node.js는 비동기 I/O를 지원, 단일 스레드 이벤트 루프 기반으로 동작함으로써 요청 처리 성능이 좋다.
> 데이터를 실시간으로 처리하기 위해 I/O 가 빈번하게 일어나는 SPA 환경에서 적합하다.
> 하지만 CPU 사용률이 높은 애플리케이션에는 권장하지 않는다.

> 자바스크립트는 크로스 플랫폼을 위한 가장 중요한 언어로 주목받고 있다.

## 느낀 점
NODE.JS는 신이고.. 나는 무적이다..

저도 백엔드, 프론트엔드 SPA 환경 등 모든 환경이 자바스크립트 환경에서 개발되다보니 너무 편한것 같습니다.

## 2.4~5 ECMAScript 와 자바스크립트, 특징

> 자바스크립트는 기본 뼈대를 이루는 ECMAScript 와 브라우저가 별도 지원하는 클라이언트 사이드 Web API로 이루어져있다.

> 웹 브라우저에서 동작하는 유일한 프로그래밍 언어이다.

> 자바스크립트는 인터프리터 언어이다.

> 자바스크립트는 명령형, 함수형, 프로토타입 기반, 객채지향 프로그래밍을 지원한다.
> 멀티 패러당미 프로그래밍 언어이다.

> 자바스크립트는 클래스 기반 객체지향 언어보다 효율적이고 강력한 프로토타입 기반의 객체지향 언어이다.

## 느낀 점
객체지향에 엄청난 이득이 있다고 하는데 지금까지 객채지향의 SOLID 논리 중

프론트에선 SRP, OCP 기능을 중심적으로 본 것 같았는데. 조금 더 객체지향적인 부분을 찾아서 적용할 기회가 있으면 좋겠다.

프론트엔드에서 캡슐화, 상속, 추상화 를 어떻게 해야할까?
해당 내용을 같이 한번 논의해봤으면 좋겠습니다.

## 2.6 ES6 브라우저 지원 현황

> 인터넷 익스폴로러를 제외한 모던 브라우저의 ES6 지원 비율은 96~99%이다.
> 구형 브라우저는 대부분 ES6를 지원하지 않는다.

> 구형 브라우저를 고려할 상황이라면 바벨과 같은 트랜스파일러를 사용해 ES6 이상의 사양으로 구현한 소스코드를 ES5 이하의 사양으로 다운그레이드 해야할 상황이 온다.

## 느낀 점

Webpack을 직접 건든적은 있는데 바벨을 아예 건든적이 없어서;; 49장 딱 기다려라잉

---

[ ] 자바스크립트의 특징에 대해서 서술해보시오.

[ ] ES6 이상의 문법 중 많이 사용하고 있는 문법을 5가지 서술해보시오.

