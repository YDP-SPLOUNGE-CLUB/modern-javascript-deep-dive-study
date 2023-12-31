# 4.1 변수란 무엇인가? 왜 필요한가?

컴퓨터는 CPU를 사용해 연산하고, 메모리를 사용해 데이터를 기억한다.

메모리는 데이터를 저장할 수 있는 메모리 셀의 집합체다.
메모리 셀 하나의 크기는 1바이트(8비트)이며
컴퓨터는 메모리 셀의 크기 1바이트 단위로 데이터를 저장하거나 읽어들인다.

각 셀은 고유의 메모리 주소를 갖는다.
메모리 주소는 메모리 공간의 위치를 나타내며 정수로 표현된다.

컴퓨터는 모든 데이터를 2진수로 처리한다.
때문에 메모리에 저장되는 모든 데이터는 2진수로 저장된다.

>자바스크립트는 직접적인 메모리 제어를 허용하지 않는다.

자바스크립트가 메모리 제어를 허용하더라도 문제가 있다.
값이 저장될 메모리 주소는 코드가 실행될 때 메모리의 상황에 따라 임의로 결정된다.
동일한 컴퓨터에서 동일한 코드를 실행해도 코드가 실행될 때마다 값이 저장될 메모리 주소는 변경된다.
이처럼 코드가 실행되기 이전에는 값이 저장된 메모리 주소를 알 수 없으며, 알려주지도 않는다.

**변수의 정의**
>변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다.

즉 값의 위치를 가리키는 상징적인 이름이다.

변수는 프로그래밍 언어의 컴파일러 또는 인터프리터에 의해 값이 저장된 메모리 공간의 주소로 치환되어 실행된다.
따라서 개발자가 직접 메모리 주소를 통해 값을 저장하고 참조할 필요가 없고 변수를 통해 안전하게 값에 접근할 수 있다.

```js
// 04-03
var result = 10 + 20;
```
연산을 통해 생성된 값 30은 메모리 공간에 저장된다.
이때 메모리 공간에 저장된 30을 다시 읽어 들여 재사용할 수 있도록 값이 저장된 메모리 공간에 상징적인 이름을 붙인 것이 바로 변수다.

**변수이름(변수명)** : 메모리 공간에 저장된 값을 식별할 수 있는 고유한 이름
**변수 값** : 변수에 저장된 값
**할당 (대입, 저장)** : 변수에 값을 저장하는 것
**참조** : 변수에 저장된 값을 읽어 들이는 것

# 4.2 식별자
변수 이름을 식별자(identifier) 라고도 한다.
**식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다.**

식별자는 메모리 공간에 저장되어 있는 어떤 값을 구별해서 식별해낼 수 있어야 한다.
이를 위해 식별자는 어떤 값이 저장되어 있는 메모리 주소를 기억(저장)해야 한다.

식별자는 값이 저장되어 있는 메모리 주소와 매핑 관계를 맺으며, 이 매핑 정보도 메모리에 저장되어야 한다.

이처럼 **식별자는 값이 아니라 메모리 주소를 기억하고 있다.**

식별자는 변수에만 국한해서 사용하지 않고, 변수, 함수, 클래스 등의 이름은 모두 식별자다.
즉 메모리 상에 존재하는 어떤 값을 식별할 수 있는 이름은 모두 식별자라고 부른다.

# 4.3 변수 선언
변수 선언이란 변수를 생성하는 것을 말한다.

값을 저장하기 위한 메모리 공간을 확보하고 변수 이름과 확보된 메모리 공간의 주소를 연결해서 값을 저장할 수 있게 준비하는 것이다.

변수 선언에 의해 확보된 메모리 공간은 확보가 해제되기 전까지는 누구도 확보된 메모리 공간을 사용할 수 없도록 보호되므로 안전하게 사용할 수 있다.

### var, let, const
> var 의 단점
> 블록 레벨 스코프를 지원하지 않고, 함수 레벨 스코프를 지원한다는 것이다.
> 이로 인해 의도치 않게 전역변수가 선언되어 심각한 부작용을 발생시킨다.
> let과 const키워드를 도입한 이유는 var 키워드의 여러 단점을 보완하기 위해서다.

```js
// 04-04
var score; // 변수 선언 (변수 선언문)
```
- 변수 이름을 등록하고 값을 저장할 메모리 공간을 확보
- 변수 선언 이후 변수에 값을 할당하지 않았다.
- 확보된 메모리 공간에는 자바스크립트 엔진에 의해 undefined라는 값이 암묵적으로 할당되어 초기화 된다.

자바스크립트 엔진은 변수 선언을 2단계에 거쳐 수행
- **선언 단계**: 변수 이름을 등록해서 자바스크립트 엔진에 변수의 존재를 알린다.
- **초기화 단계**: 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다.

### 변수이름은 어디에 등록 될까?
>변수 이름을 비롯한 모든 식별자는 실행 컨텍스트에 등록된다.
>실행 컨텍스트는 자바스크립트 엔진이 소스코드를 평가하고 실행하기 위해 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.
>자바스크립트 엔진은 실행 컨텍스트를 통해 식별자와 스코프를 관리한다.
>
>변수 이름과 변수 값은 실행 컨텍스트 내에 key/value 형식인 객체로 등록되어 관리된다.

만약 초기화 단계를 거치지 않으면 확보된 메모리 공간에는 이전에 다른 애플리케이션이 사용했던 값이 남아 있을 수 있다.
이러한 값을 쓰레기 값(garbage value)이라 한다.
따라서 메모리 공간을 확보한 다음, 값을 할당하지 않은 상태에서 곧바로 변수 값을 참조하면 쓰레기 값이 나올 수 있다.

> var 키워드는 암묵적으로 초기화를 수행하므로 이러한 위험으로부터 안전하다.

변수를 선언하지 않은 식별자에 접근하면 **ReferenceError(참조에러)** 가 발생한다.

# 4.4 변수 선언의 실행 시점과 변수 호이스팅
```js
// 04-05
console.log(score); // undefined
var score; // 변수 선언문
```

console.log(score) 가 실행되는 시점에 참조 에러가 발생할 것 같지만 undefined 가 호출된다.

그 이유는 **변수 선언이 소스코드가 한 줄씩 순차적으로 실행되는 시점, 즉 런타임이 아니라 그 이전단계에서 먼저 실행되기 때문이다.**

자바스크립트 엔진은 소스코드를 한 줄씩 순차적으로 실행하기에 앞서 먼저 소스코드의 평가 과정을 거치면서 소스코드를 실행하기 위한 준비를 한다.
이때 소스코드 실행을 위한 준비 단계인 소스코드의 평가 과정에서 변수 선언을 포함한 모든 선언문(변수 선언문, 함수 선언문 등)을 소스코드에서 찾아내 먼저 실행한다.
소스코드 평가 과정이 끝나면 비로소 모든 선언문을 제외하고 소스코드를 한 줄씩 순차적으로 실행한다.

>즉 자바스크립트 엔진은 변수 선언이 소스코드의 어디에 있든 상관없이 다른 코드보다 먼저 실행한다
>따라서 변수 선언이 소스코드의 어디에 위치하는지와 상관없이 어디서든지 변수를 참조할 수 있다.

**변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 변수 호이스팅**이라 한다.

사실 변수 선언뿐 아니라 **모든 식별자는 호이스팅 된다.**
- var
- let
- const
- function
- function*
- class
  모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문이다.

# 4.5 값의 할당

```js
// 04-06
var score; // 변수 선언
score = 80; // 값의 할당
```

```js
// 04-07
var score = 80; // 변수 선언과 값의 할당
```

위 두 코드는 정확히 동일하게 동작한다.

> 자바스크립트 엔진은 변수 선언과 값의 할당을 하나의 문으로 단축 표현해도
> **변수 선언과 값의 할당을 2개의 문으로 나누어 각각 실행한다.**

이때 주의할 점은 변수 선언과 값의 할당의 실행 시점이 다르다는 것이다.

**변수 선언**은 소스코드가 순차적으로 실행되는 시점인 **런타임 이전에 먼저 실행**되지만
**값의 할당**은 소스코드가 순차적으로 실행되는 시점인 **런타임에 실행**된다.

```js
// 04-8
console.log(score); // undefined

// 런타임 이전에 먼저 실행된다
var score; // 변수 선언
// 값의 할당은 런타임에 실행된다. undefined 에서 80으로 변경(재할당)된다.
score = 80; // 값의 할당

console.log(score); // 80
```

변수에 값을 할당할 때는 이전 undefined 저장되어 있던 메모리 공간을 지우고 그 메모리 공간에 할당 값 80을 새롭게 저장하는 것이 아니라 새로운 메모리 공간을 확보하고 그곳에 할당 값 80을 저장한다.

```js
// 04-10
console.log(score); // undefined

score = 80;
var score;

console.log(score); // 80
```

# 4.6 값의 재할당
재할당이란 이미 값이 할당되어 있는 변수에 새로운 값을 또다시 할당하는 것을 말한다.
```js
// 04-11
var score = 80; // 변수 선언과 값의 할당
score = 90; // 값의 재할당
```

**변수에 저장된 값을 변경할 수 없다면 변수가 아니라 상수(constant)라 한다.**
상수는 단 한번만 할당할 수 있는 변수이다.

값을 재할당하면 이전 값 80이 저장되어 있던 메모리 공간을 지우고 그 메모리 공간에 재할당 값 90을 새롭게 저장하는 것이 아니라 새로운 메모리 공간을 확보하고 그 메모리 공간에 숫자 값 90을 저장한다.

score 변수의 이전 값인 undefined 와 80은 더이상 사용하고 있지 않다.
이러한 불필요한 값들은 **가비지 콜렉터**에 의해 메모리에서 자동 해제된다.
단 메모리에서 언제 해제될지는 예측할 수 없다.

### 가비지콜렉터
>가비지 콜렉터는 애필리케이션이 할당한 메모리 공간을 주기적으로 검사하여 더 이상 사용되지 않는 메모리를 해제하는 기능을 말한다.
>더 이상 사용되지 않는 메모리란 간단히 말하자면 어떤 식별자도 참조하지 않는 메모리 공간을 의미한다.
>자바스크립트는 가비지 콜렉터를 내장하고 있는 매니지드 언어로서 가비지 콜렉터를 통해 메모리 누수를 방지한다.

### 언매니지드 언어 와 매니지드 언어
> 프로그래밍 언어는 메모리 관리 방식에 따라 언매니지드 언어와 매니지드 언어로 분류할 수 있다.

> 언매니지드 언어는 개발자가 명시적으로 메모리를 할당하고 해제하기 위해 malloc()과 free() 같은 저수준 메모리 제어 기능을 제공한다.
> 언매니지드 언어는 메모리 제어를 개발자가 주도할 수 있으므로 개발자의 역량에 따라 최적의 성능을 확보할 수 있지만 그 반대의 경우 치명적인 오류를 생산할 가능성도 있다.
> ex: C 언어

> 매니지드 언어는 메모리의 할당 및 해제를 위한 메모리 관리 기능을 언어 차원에서 담당하고 개발자의 직접적인 메모리 제어를 허용하지 않는다.
> 즉 개발자가 명시적으로 메모리를 할당하고 해제할 수 없다.
> 더 이상 사용하지 않는 메모리의 해제는 가비지 콜렉터가 수행하며, 이또한 개발자가 관여할 수 없다.
> 매니지드 언어는 개발자의 역량에 의존하는 부분이 상대적으로 작아져 어느 정도 일정한 생산성을 확보할 수 있다는 장점이 있지만 성능 면에서 어느 정도의 손실을 감수할 수 밖에 없다.
> ex: JavaScript

# 4.7 식별자 네이밍 규칙
앞에서 언급했듯이 식별자는 어떤 값을 구별해서 식별해낼 수 있는 고유한 이름을 말한다.

네이밍 규칙
- 식별자는 특수문자를 제외한 문자, 숫자, 언더스코어, 달러 기호를 포함할 수 있다.
- 단, 식별자는 특수문자를 제외한 문자, 언더스코어, 달러 기호로 시작해야 한다. 숫자로 시작하는 것은 허용하지 않는다.
- 예약어는 식별자로 사용할 수 없다.

예약어 : 프로그래밍 언어에서 사용되고 있거나 사용될 예정인 단어를 말한다.
ex : await, if, let, new, etc...

```js
// 04-12 여러 개 한번에 선언할 수 있다. 하지만 가독성이 나빠지므로 권장하지 않음.
var person, $elem, _name, first_name, val1;

// 04-13 알파벳 외의 유니코드 문자는 바람직하지 않다.
var 이름

// 04-14 명명 규칙에 위배된다.
var first-name; // SyntaxError
var 1st; // SyntaxError
var this; // SyntaxError

// 04-15 자바스크립트는 대소문자를 구별하므로 다음 변수는 각각 별개의 변수다.
var firstname;
var firstName;
var FIRSTNAME;

// 04-16 좋은 변수 이름은 코드의 가독성을 높인다.
var x = 3; // NG. X 변수가 의미하는 바를 알 수 없음.
var score = 100; // Ok. Score 변수는 점수를 의미한다.

// 04-17 별도의 주석이 필요하다면 존재 목적을 명확히 드러내지 못하는 것.
// 경과 시간, 단위는 날짜다
var d; // NG
var elapsedTimeInDays; // Ok

// 04-18
// 카멜 케이스(camelCase)
var firstName;
// 스네이크 케이스(snake_case)
var first_name;
// 파스칼 케이스(PascalCase)
var FirstName;
// 헝가리언 케이스(typeHungarianCase)
var strFirstName;
var $elem = document.getElementById('myId');
var observable$ = fromEvent(docuemnt, 'click');
```

네이밍 컨벤션 같은 경우 일반적으로 변수나 함수의 이름에는 카멜 케이스를 사용하고,
생성자 함수, 클래스의 이름에는 파스칼 케이스를 사용한다.

ECMAScript 사양에 정의되어 있는 객체와 함수들도 카멜 케이스와 파스칼 케이스를 사용한다.

# 알고 가기
---
- [ ] 아래 두 가지 코드의 동작에 대하여 설명하시오.

```js
var score;
score = 80;
score = 90;
```

```js
var score = 80;
score = 90;
```
