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

| 버전   | 출시 연도 | 특징                                                                                                                         |                             
|------|-------|----------------------------------------------------------------------------------------------------------------------------|
| ES1  | 1997  | 초판                                                                                                                         |
| ES2  | 1998  | ISO/IEC 16262 국제 표준과 동일한 규격 적용                                                                                             |
| ES3  | 1999  | 정규 표현식, try catch                                                                                                          |
| ES5  | 2009  | HTML와 함께 출현한 표준안, JSON, strict mode, 접근자 프로퍼티, 프로퍼티 어트리뷰트 제어, 향상된 배열, 조작 기능 (forEach map, filter, reduce, some, every)     |
| ES6  | 2015  | let/const, 화살표, 템플릿 리터널, 디스트럭처링 할당, 스프레드, rest 파라미터, 심벌, 프로미스, Map/Set 이터러블 for ... of, 제너레이터, Proxy, 모듈 import/export     |
| ES7  | 2016  | 지수 연산자, Array, prototype, includes, String.prototype.includes                                                              |
| ES8  | 2017  | async/await Object 정적 메서드(Object.values, Object.entries, Object.getOwnPropertyDescriptors)                                 |
| ES9  | 2018  | Obejct rest/spread 프로퍼티, Promise.prototype, finally, async generator, for await ... of                                     |
| ES10 | 2019  | Object.formEntries, Array.prototype.flat, Array.prototype.flatMap, optional catch binding                                  |
| ES11 | 2020  | String.prototype.matchAll, BingInt, globalThis, Promise.allSettled, null 병합 연산자, 옵셔널 체이닝 연산자, for ... in enumeration order |

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


| 버전   | 출시 연도 | 특징                                                                                   |                             
|------|-------|--------------------------------------------------------------------------------------|
| ES13 | 2022  | 퍼블릭 핃드 및 정적 필드 선언 방식 개선, 프라이빗 접근 제어자 추가, 정적 초기화 블록 추가, in 연산자, 최상위 레벨 await 모듈 호출 가능 |




