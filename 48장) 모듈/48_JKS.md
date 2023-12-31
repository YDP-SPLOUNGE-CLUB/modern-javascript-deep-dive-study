# 48장 모듈

## 48.1 모듈의 일반적 의미

> 모듈이란 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각을 말한다.
> 일반적으로 모듈은 기능을 기준으로 파일 단위로 분리한다.

> 자신만의 파일 스코프를 갖는 모듈의 자산은 기본적으로 비공개 상태다. 자신만의 파일 스코프를 갖는 모듈의 모든 자산은 캡슐화되어 다른 모듈에서 접근 불가능하다.

> 하지만 애플리케이션과 완전히 분리되어 개별적으로 존재하는 모듈은 재사용이 불가능하므로 존재의 의미가 없다. 모듈은 애플리케이션이나 다른 모듈에 의해 재사용되어야
> 의미가 있다. 모듈은 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개가 가능하다. 이를 export 라 한다.

> 모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있다 이를 import 라 한다.


## 48.2 자바스크립트와 모듈

> 자바스크립트는 웹페이지의 단순한 보조 기능을 처리하기 위한 제한적인 용도를 목적으로 태어났다.
> 이러한 태생적 한계로 다른 프로그래밍 언어와 비교할 때 부족한 부분이 있는 것이 사실이다. 대표적인 것이 모듈 시스템을 지원하지 않는다는 것이다.
> 다시 말해, 자바스크립트는 모듈이 성립하기 위해 필요한 파일 스코프와 import , export 를 지원하지 않았다.

> 모든 자바스크립트 파일은 하나의 전역을 공유한다. 분리된 자바스크립트 파일들의 전역 변수가 중복되는 등의 문제가 발생할 수 있다.

> 자바스크립트를 클라이언트 사이드 브라우저 환경에 국한하지 않고 범용적으로 사용하려는 움직임이 생기면서 모듈 시스템은 반드시 해결해야 하는 핵심 과제가 되었다.
> 이런 상황에서 제안된 것이 CommonJs 와 AMD 다.

> 자바스크립트 런타임 환경인 Node.js 는 모듈 시스템의 사실상 표준인 CommonJS 를 채택하고 독자적인 진화를 거쳐 현재는
> CommonJS 사양과 100% 동일하지는 않지만 기본적으로 CommonJS 사양을 따르고 있다.

## 48.3 ES6 모듈 (ESM)

> 이러한 상황에서 ES6 에서는 클라이언트 사이드 자바스클비트에서도 동작하는 모듈 기능을 추가했다. IE 를 제외하는 대부분의 브라우저애서 ES6 모듈을 사용할 수 있다.

```javascript
<script>type="module" src="app.mjs"></scropt>
```

### 48.3.1 모듈 스코프

> ESM 은 독자적인 모듈 스코프를 갖는다. ESM 이 아닌 일반적인 자바스크립트 파일은 script 태그로 분리해서 로드해도 독자적인 모듈 스코프를 갖지 않는다.

```javascript
var x = 'foo';

console.log(x); // foo
console.log(window.x); // undefined
```

### 48.3.2 export 키워드

> 모듈은 독자적인 모듈 스코프를 갖는다. 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 사용 가능하다.
> 다른 모듈들이 재사용 가능하도록 하려면 export 키워드를 사용한다.

### 48.3.3 import 키워드

> 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하기 위해서 import 키워드를 사용한다.

