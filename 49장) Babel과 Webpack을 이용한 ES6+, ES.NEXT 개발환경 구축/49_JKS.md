# Babel과 Webpack을 이용한 ES6+,ES.NEXT 개발 환경 구축

> 크롬 사파리 파이어폭스 엣지 같은 에버그린 브라우저의 ES6 지원율은 약 98% 로 거의 대부분의 ES6 를 지원한다.
> 매년 새롭게 도입되는 ES6 이상의 버전과 제안 단계에 있는 ES 제안 사양은 브라우저에 따라 지원율이 제각각이다.

> ES6+ ES.NEXT 의 최신 ECMAScript 사양을 사용하여 프로젝트를진행하려면 최신 사양으로 작성된 코드를 경우에 따라 구형 브라우저에서 문제 없이 동작시키기 위한
> 개발환경을 구축하는 것이 필요하다.

> 또한 대부분의 폴젝트가 모듈을 사용하므로 모듈 로더또한 필요하다.

## 49.1 Babel

```javascript
[1,2,3].map(n => n ** n);
```

> 이 코드는 IE 같은 구형 브라우저에서 동작하지 않는다. Babel 을 사용하면 위 코드를 다음과 같은 ES5 사양으로 변환 가능하다.

```javascript
'use strict';

[1,2,3].map(function (n) {
    return Math.pow(n,n);
})
```

## 49.2 Webpack

> 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링하는 모듈 번들러이다. Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로
> 별도의 모듈 로더가 필요 없다. 그리고 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.

### 49.2.4 babel-polyfill

> Babel 을 사용하여 ES6+/ ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지우너하지 않는 코드가 남아 있을 수 있다.
> ES6 에서 추가된 Promise, Object.assign, Array.from 등은 ES5 사양으로 트랜스파일링해도 대채할 기능이 없기 떄문에 트랜스파일링되지 못한다.

> 이를 사용하기 위해서 babel/polyfill 을 설치해야한다.

---

생략
